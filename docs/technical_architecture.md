## VenturePilot AI: Comprehensive Technical Architecture

This document outlines the technical architecture for VenturePilot AI, a SaaS platform built with Next.js, Convex, and Vercel. It covers all aspects of the application, from frontend design to backend implementation, deployment, and optimization, adhering to the project requirements and the mandate to use only the specified technologies.

**1. Frontend Architecture (Next.js 14+)**

*   **App Router Structure and Page Organization:**

    *   We will use the Next.js App Router for a modern, performant, and flexible structure.
    *   **`/app` directory:**
        *   `layout.tsx`:  The root layout, defining the overall structure (header, footer, etc.).
        *   `page.tsx`:  The home page (landing page).
        *   `dashboard/`:  Protected routes for authenticated users.
            *   `layout.tsx`:  Dashboard layout (sidebar, content area).
            *   `page.tsx`:  Dashboard overview.
            *   `reports/`:
                *   `page.tsx`:  Reports list.
                *   `[reportId]/page.tsx`:  Report detail page.
            *   `market-monitor/`:
                *   `page.tsx`:  Market Monitor interface.
            *   `idea-pivot/`:
                *   `page.tsx`:  Idea Pivot Recommender interface.
            *   `ai-coach/`:
                *   `page.tsx`:  Founder-to-Market-Fit AI Coach interface.
            *   `profile/`:
                *   `page.tsx`:  User profile.
            *   `admin/`: (Protected by admin role)
                *   `layout.tsx`:  Admin dashboard layout.
                *   `users/page.tsx`:  User management.
                *   `analytics/page.tsx`:  Analytics dashboard.
                *   `content/page.tsx`:  Content management.
                *   `settings/page.tsx`:  Application settings.
        *   `auth/`:
            *   `login/page.tsx`:  Login page.
            *   `signup/page.tsx`:  Signup page.
            *   `forgot-password/page.tsx`:  Password reset.
        *   `api/`:  (API routes for Stripe webhooks, etc.)

*   **Server Components vs. Client Components Strategy:**

    *   **Server Components (default):**  Used extensively for data fetching, rendering static content, and creating the initial HTML.  Data fetching will primarily be done using Convex queries.
    *   **Client Components ( `“use client”` directive):**  Used for interactivity, state management, and components that require browser APIs (e.g., file upload, form handling, real-time updates).

*   **State Management Approach:**

    *   **React Hooks:**  Leverage React Hooks (e.g., `useState`, `useEffect`, `useContext`) for managing local component state and side effects.
    *   **Convex React Hooks:**  Use Convex React hooks (`useQuery`, `useMutation`, `useSubscription`) for fetching, updating, and subscribing to real-time data from Convex.
    *   **Zustand (optional):**  For more complex state management needs across multiple components, consider using Zustand, a lightweight and performant state management library.  This is particularly useful for global application state like user authentication status.

*   **Real-time Data Handling with Convex React Hooks:**

    *   `useQuery`: For fetching data from Convex.
    *   `useMutation`: For triggering Convex mutations (e.g., creating a report, updating user profile).
    *   `useSubscription`: For subscribing to real-time updates from Convex, such as updates to the market monitor or new notifications.

*   **Responsive Design System and Component Library:**

    *   **UI Library:**  Choose a UI library like Chakra UI, Ant Design, or Material UI (or build a custom one). This provides pre-built, accessible, and customizable components.
    *   **Responsive Design:**  Use the UI library's responsive features (e.g., grid systems, breakpoints) to ensure the application is usable on all devices. Implement responsive images and layouts.

    **Example: Report Detail Page (Server Component)**

    ```typescript
    // app/dashboard/reports/[reportId]/page.tsx (Server Component)
    import { useQuery } from 'convex/react';
    import { api } from '@/convex/_generated/api';
    import { Report } from '@/types/report';
    import ReportDetailHeader from '@/components/ReportDetailHeader';
    import ReportContent from '@/components/ReportContent';

    interface Props {
      params: { reportId: string };
    }

    export default async function ReportDetailPage({ params }: Props) {
      const reportId = params.reportId;
      const report = await useQuery(api.reports.getReport, { reportId });

      if (!report) {
        return <div>Loading...</div>; // Or a more sophisticated loading state
      }

      return (
        <>
          <ReportDetailHeader report={report} />
          <ReportContent report={report} />
        </>
      );
    }
    ```

    **Example: Report Content (Client Component - for interactivity)**

    ```typescript
    // components/ReportContent.tsx (Client Component)
    "use client";
    import { Report } from '@/types/report';
    import { useMutation } from 'convex/react';
    import { api } from '@/convex/_generated/api';
    import { useState } from 'react';

    interface Props {
      report: Report;
    }

    export default function ReportContent({ report }: Props) {
      const [isSaving, setIsSaving] = useState(false);
      const updateReport = useMutation(api.reports.updateReport);

      const handleSave = async () => {
        setIsSaving(true);
        try {
          await updateReport({ reportId: report._id, .../* edited fields */ });
          // Show success message
        } catch (error) {
          // Show error message
        } finally {
          setIsSaving(false);
        }
      };

      return (
        <div>
          {/* Render report data here */}
          <button onClick={handleSave} disabled={isSaving}>
            {isSaving ? 'Saving...' : 'Save Changes'}
          </button>
        </div>
      );
    }
    ```

**2. Backend Architecture (Convex)**

*   **Database Schema Design:**

    ```typescript
    // convex/schema.ts
    import { defineSchema, defineTable } from 'convex/server';
    import { v } from 'convex/values';

    export const schema = defineSchema({
      users: defineTable({
        auth0Id: v.string(), // Convex Auth ID
        email: v.string(),
        name: v.string(),
        profileImage: v.optional(v.string()), // URL to profile image
        stripeCustomerId: v.optional(v.string()), // Stripe customer ID
        role: v.optional(v.union(v.literal('admin'), v.literal('user'))),
      }).index('by_auth0Id', ['auth0Id']),

      reports: defineTable({
        userId: v.id('users'),
        idea: v.string(),
        marketSize: v.optional(v.string()), // Store as string for flexibility
        competitors: v.optional(v.array(v.string())),
        customerPainPoints: v.optional(v.array(v.string())),
        gmtStrategy: v.optional(v.string()),
        status: v.optional(v.union(v.literal('pending'), v.literal('complete'), v.literal('failed'))),
        reportData: v.optional(v.any()), // Store AI generated data
        fileId: v.optional(v.id('_storage')), // Link to file if generated
      }).index('by_userId', ['userId']),

      marketMonitorAlerts: defineTable({
        reportId: v.id('reports'),
        alertType: v.string(), // e.g., "New Competitor", "Trend Shift"
        description: v.string(),
        createdAt: v.number(),
      }).index('by_reportId', ['reportId']),

      ideaPivotSuggestions: defineTable({
        reportId: v.id('reports'),
        suggestion: v.string(),
        rationale: v.string(),
        createdAt: v.number(),
      }).index('by_reportId', ['reportId']),

      notifications: defineTable({
        userId: v.id('users'),
        type: v.string(), // e.g., "report_complete", "market_alert"
        message: v.string(),
        read: v.boolean(),
        data: v.optional(v.any()), // Additional data for the notification
        createdAt: v.number(),
      }).index('by_userId', ['userId']),

      // Stripe Events (for webhook handling)
      stripeEvents: defineTable({
        event_id: v.string(),
        type: v.string(),
        data: v.any(),
      }).index('by_event_id', ['event_id'])
    });
    ```

*   **Query and Mutation Function Organization:**

    *   Organize Convex functions into modules based on functionality (e.g., `users.ts`, `reports.ts`, `stripe.ts`, `marketMonitor.ts`).
    *   Use clear and descriptive function names.
    *   Implement input validation in all functions using `zod` or similar libraries (see below).

    ```typescript
    // convex/users.ts
    import { mutation, query } from 'convex/server';
    import { v } from 'convex/values';
    import { z } from 'zod';

    const createUserInput = z.object({
      auth0Id: z.string(),
      email: z.string().email(),
      name: z.string(),
    });

    export const createUser = mutation({
      args: createUserInput.shape,
      handler: async (ctx, args) => {
        const existingUser = await ctx.db
          .query('users')
          .withIndex('by_auth0Id', (q) => q.eq('auth0Id', args.auth0Id))
          .first();

        if (existingUser) {
          return existingUser._id;
        }

        const userId = await ctx.db.insert('users', args);
        return userId;
      },
    });
    ```

*   **Action Functions for External API Integrations:**

    *   Use Convex `action` functions to interact with external APIs (e.g., AI model, Stripe, email service).
    *   Actions run on the server and can securely handle API keys and other sensitive information.

    ```typescript
    // convex/reports.ts
    import { mutation, action } from 'convex/server';
    import { v } from 'convex/values';
    import { z } from 'zod';
    import { api } from './_generated/api';

    const generateReportInput = z.object({
      userId: z.id('users'),
      idea: z.string(),
    });

    export const generateReport = mutation({
      args: generateReportInput.shape,
      handler: async (ctx, args) => {
        // 1. Insert a new report with status 'pending'
        const reportId = await ctx.db.insert('reports', {
          userId: args.userId,
          idea: args.idea,
          status: 'pending',
        });

        // 2. Queue an action to process the report using the AI model
        await ctx.scheduler.runAfter(0, api.reports.processReport, {
          reportId,
        });

        return reportId;
      },
    });

    // convex/reports.ts
    import { action } from 'convex/server';
    import { v } from 'convex/values';
    import { z } from 'zod';
    import { Configuration, OpenAIApi } from "openai";
    import { api } from './_generated/api';

    const processReportInput = z.object({
      reportId: z.id('reports'),
    });

    export const processReport = action({
      args: processReportInput.shape,
      handler: async (ctx, args) => {
        const report = await ctx.db.get(args.reportId);
        if (!report || report.status !== 'pending') {
          console.log("Report not found or not pending", args.reportId);
          return;
        }

        // 1. Call OpenAI API (or other AI service)
        const configuration = new Configuration({
          apiKey: process.env.OPENAI_API_KEY,
        });
        const openai = new OpenAIApi(configuration);

        const completion = await openai.createChatCompletion({
          model: "gpt-3.5-turbo",
          messages: [{
            role: "user",
            content: `Analyze the following startup idea: ${report.idea}. Generate a report.  Include market size, competitor analysis, and customer pain points.`,
          }],
        });

        const reportData = completion.data.choices[0].message?.content;
        if (!reportData) {
          console.error("No report data received from OpenAI");
          await ctx.db.patch(args.reportId, { status: 'failed' });
          return;
        }

        // 2. Update the report with the AI-generated data
        await ctx.db.patch(args.reportId, {
          reportData,
          status: 'complete',
        });

        // 3. Trigger market monitor updates
        await ctx.scheduler.runAfter(0, api.marketMonitor.startMonitoring, {
          reportId: args.reportId,
        });
      },
    });
    ```

*   **Real-time Subscription Patterns:**

    *   Use Convex subscriptions for real-time updates:
        *   **Report Status Updates:** Subscribe to updates on the `reports` table.
        *   **Market Monitor Alerts:** Subscribe to updates on the `marketMonitorAlerts` table.
        *   **Notifications:** Subscribe to updates on the `notifications` table.

    ```typescript
    // Example: Subscribe to report updates in a client component
    "use client";
    import { useSubscription } from 'convex/react';
    import { api } from '@/convex/_generated/api';
    import { Report } from '@/types/report';

    interface Props {
      reportId: string;
    }

    export default function ReportStatus({ reportId }: Props) {
      const report = useSubscription(api.reports.getReport, { reportId });

      if (!report) {
        return <div>Loading...</div>;
      }

      return (
        <div>
          Report Status: {report.status}
        </div>
      );
    }
    ```

*   **File Storage Architecture:**

    *   Use Convex file storage for storing files uploaded by users (e.g., profile images, generated reports in PDF format).
    *   Convex handles file uploads, storage, and CDN delivery (via Vercel).
    *   Use the `upload()` function to upload files and the `getFileUrl()` function to retrieve the file URL.

    ```typescript
    // convex/reports.ts
    import { mutation } from 'convex/server';
    import { v } from 'convex/values';
    import { api } from './_generated/api';
    import { z } from 'zod';
    import { File } from 'convex/storage';

    const uploadReportFileArgs = z.object({
      reportId: z.id('reports'),
      file: v.optional(v.file()),
    });

    export const uploadReportFile = mutation({
      args: uploadReportFileArgs.shape,
      handler: async (ctx, args) => {
        if (!args.file) {
          return;
        }

        const fileId = await ctx.storage.store(args.file);
        await ctx.db.patch(args.reportId, { fileId });

        return fileId;
      },
    });
    ```

**3. Standard Feature Implementation**

*   **Admin Dashboard:**

    *   **Admin-only routes and components:**  Use Convex Auth to determine user roles.  Restrict access to admin routes using server-side checks (e.g., in `getServerSideProps` or in server components).
    *   **User management and analytics interfaces:**  Admin dashboard components to view, edit, and manage users.  Analytics components to visualize application usage data.
    *   **Analytics data collection and aggregation:**  Use Convex queries to aggregate data for the analytics dashboard.  Log events (e.g., report creation, user logins) in Convex.
    *   **Dashboard components for data visualization:**  Use charting libraries (e.g., Recharts, Chart.js) to display analytics data.
    *   **Export functionality for reports:**  Allow admins to export user data, reports, etc. (e.g., CSV, JSON).

*   **API Integration:**

    *   Define API routes within the `/app/api` directory.
    *   Handle API requests using server actions.
    *   Use Convex actions to interact with external APIs (e.g., OpenAI, payment gateways).

*   **Content Management:**

    *   Create a content management interface within the admin dashboard.
    *   Store content (e.g., blog posts, FAQs)