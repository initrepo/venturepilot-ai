## VenturePilot AI: UX/UI Design System Document

This document outlines the UX/UI design system for VenturePilot AI, a SaaS platform providing AI-driven market intelligence reports for startup founders. It's designed to guide developers in building a modern, user-friendly, and scalable application using Next.js, Tailwind CSS, and Vercel.

---

## 1. Design System Foundation

### **1.1 Brand Identity**

*   **Logo Concepts:**
    *   **Option 1 (Abstract):** A stylized "V" and "P" intertwined, forming a dynamic shape suggesting growth and insight. Colors: Primary Blue and a subtle accent color.
    *   **Option 2 (Iconic):** A lightbulb integrated with a magnifying glass, symbolizing innovation and market analysis. Colors: Primary Blue and a secondary, lighter blue.
*   **Brand Personality:**
    *   **Trustworthy:** Reliable, data-driven, and professional.
    *   **Intelligent:** Sophisticated, AI-powered, and insightful.
    *   **Empowering:** Supportive, helpful, and enabling.
    *   **Modern:** Clean, sleek, and forward-thinking.
*   **Visual Identity Guidelines:**
    *   **Logo Usage:** Clear space around the logo, minimum size specifications, and acceptable variations (e.g., monochrome versions).
    *   **Brand Voice:** Tone of voice should be informative, helpful, and confident. Avoid overly technical jargon.

### **1.2 Color System**

*   **Primary Color:** `#2563EB` (Blue-600) - Used for primary actions, headings, and interactive elements.
*   **Secondary Color:** `#60A5FA` (Blue-300) - Used for accents, highlights, and secondary actions.
*   **Tertiary Color:** `#E0F2FE` (Blue-50) - Used for backgrounds, subtle highlights, and subtle accents.
*   **Success Color:** `#16A34A` (Green-600) - Used for success messages, positive feedback.
*   **Warning Color:** `#FACC15` (Yellow-400) - Used for warnings and attention-grabbing messages.
*   **Error Color:** `#EF4444` (Red-600) - Used for error messages and negative feedback.
*   **Neutral Colors:**
    *   `#F3F4F6` (Gray-100) - Used for light backgrounds and subtle separators.
    *   `#E5E7EB` (Gray-200) - Used for borders, dividers, and subtle backgrounds.
    *   `#D1D5DB` (Gray-300) - Used for less prominent text and subtle UI elements.
    *   `#9CA3AF` (Gray-400) - Used for disabled states, placeholder text, and secondary text.
    *   `#6B7280` (Gray-500) - Used for primary text and input labels.
    *   `#374151` (Gray-700) - Used for headings, important text, and strong UI elements.
    *   `#1F2937` (Gray-800) - Used for dark text and backgrounds.

### **1.3 Typography Scale**

*   **Font Family:**  `Inter`, sans-serif (Google Fonts).  A clean, readable font suitable for both body text and UI elements.
*   **Heading Hierarchy:**
    *   **H1:** 3rem (48px) / 1.125 line-height, bold (For page titles and main headings).
    *   **H2:** 2.25rem (36px) / 1.25 line-height, semi-bold (For section headings).
    *   **H3:** 1.5rem (24px) / 1.375 line-height, semi-bold (For subsection headings).
    *   **H4:** 1.25rem (20px) / 1.5 line-height, semi-bold (For smaller sections and labels).
    *   **H5:** 1.125rem (18px) / 1.5 line-height, semi-bold (For sub-labels and minor headings).
    *   **H6:** 1rem (16px) / 1.5 line-height, semi-bold (For less important headings).
*   **Body Text:** 1rem (16px) / 1.625 line-height, regular.
*   **Caption:** 0.875rem (14px) / 1.4 line-height, regular (For supporting text, descriptions, and metadata).
*   **Font Weights:**
    *   Regular: 400
    *   Semi-bold: 600
    *   Bold: 700

### **1.4 Spacing System**

*   **Spacing Scale (in pixels and rem):**
    *   `space-0`: 0px (0rem)
    *   `space-1`: 4px (0.25rem)
    *   `space-2`: 8px (0.5rem)
    *   `space-3`: 16px (1rem)
    *   `space-4`: 24px (1.5rem)
    *   `space-5`: 32px (2rem)
    *   `space-6`: 48px (3rem)
    *   `space-7`: 64px (4rem)

### **1.5 Border Radius**

*   **Default:** `rounded-md` (0.375rem / 6px) - Used for buttons, cards, inputs, and other UI elements.
*   **Rounded:** `rounded-full` (for circular elements)
*   **Sharp:** `rounded-none` (for specific edge cases)

### **1.6 Shadow System**

*   **Shadow 0:** `shadow-none` (No shadow)
*   **Shadow 1:** `shadow-sm` (0 1px 2px 0 rgba(0, 0, 0, 0.05)) - Subtle shadow for subtle elevation.
*   **Shadow 2:** `shadow` (0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06)) - Moderate shadow for cards and elevated elements.
*   **Shadow 3:** `shadow-md` (0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06)) - Strong shadow for prominent elements.

---

## 2. Component Library Specifications

### **2.1 Buttons**

*   **Primary:**
    *   Background: `#2563EB` (Blue-600)
    *   Text: White
    *   Padding: `py-2 px-4` (8px vertical, 16px horizontal)
    *   Border Radius: `rounded-md`
    *   Hover:  `bg-blue-700` (Darker blue)
    *   Active: `bg-blue-800` (Even darker blue) & `shadow-sm`
    *   Disabled: `bg-gray-400` (Gray-400), text: `text-gray-200`, cursor: `cursor-not-allowed`
*   **Secondary:**
    *   Background: Transparent
    *   Text: `#2563EB` (Blue-600)
    *   Border: 1px solid `#2563EB` (Blue-600)
    *   Padding: `py-2 px-4`
    *   Border Radius: `rounded-md`
    *   Hover: `bg-blue-100` (Light blue)
    *   Active: `bg-blue-200` (Medium light blue) & `shadow-sm`
    *   Disabled: `border-gray-400`, text: `text-gray-400`, cursor: `cursor-not-allowed`
*   **Outline:**
    *   Background: Transparent
    *   Text: `#374151` (Gray-700)
    *   Border: 1px solid `#D1D5DB` (Gray-300)
    *   Padding: `py-2 px-4`
    *   Border Radius: `rounded-md`
    *   Hover: `bg-gray-100` (Light gray)
    *   Active: `bg-gray-200` (Medium light gray) & `shadow-sm`
    *   Disabled: `border-gray-400`, text: `text-gray-400`, cursor: `cursor-not-allowed`
*   **Ghost:**
    *   Background: Transparent
    *   Text: `#374151` (Gray-700)
    *   Padding: `py-2 px-4`
    *   Border Radius: `rounded-md`
    *   Hover: `bg-gray-100` (Light gray)
    *   Active: `bg-gray-200` (Medium light gray) & `shadow-sm`
    *   Disabled: `text-gray-400`, cursor: `cursor-not-allowed`

### **2.2 Form Elements**

*   **Input Fields:**
    *   Border: 1px solid `#D1D5DB` (Gray-300)
    *   Border Radius: `rounded-md`
    *   Padding: `py-2 px-3`
    *   Font Size: 1rem (16px)
    *   Focus: `border-blue-500`, `shadow-sm`
    *   Placeholder Text: `#9CA3AF` (Gray-400)
    *   Error State: `border-red-500`
*   **Textareas:**  Same styles as input fields.
*   **Select Dropdowns:**  Use a custom component or library (e.g., `react-select`) for enhanced functionality and styling.  Integrate styles from input fields, including focus and error states.
*   **Checkboxes:**
    *   Appearance:  Custom styled with Tailwind.  Box: `#D1D5DB`, Checked: `#2563EB`, disabled: `cursor-not-allowed`
    *   Label:  Font size 1rem, `text-gray-700`
*   **Radio Buttons:**  Similar to checkboxes, with custom styling.

### **2.3 Navigation**

*   **Header/Navbar:**
    *   Background: White
    *   Padding: `py-4 px-6`
    *   Box Shadow: `shadow-md`
    *   Logo:  Brand logo (see Brand Identity section)
    *   Navigation Items:  Links with `text-gray-700`, hover: `text-blue-600`, active: `text-blue-700`
    *   Mobile Menu (Hamburger icon):  Icon with a menu overlay.
*   **Sidebar:**
    *   Background: `#F3F4F6` (Gray-100)
    *   Width:  256px (desktop), Collapsible (tablet and mobile)
    *   Navigation Items:  Links with `text-gray-700`, hover: `bg-gray-200`, active: `bg-gray-300`,  icons aligned to the left.
    *   Footer:  Branding, copyright information.
*   **Breadcrumbs:**
    *   Background: Transparent
    *   Text: `text-gray-500`
    *   Separator:  `/` with `text-gray-300`
    *   Current Page:  `text-blue-600`, bold.
*   **Pagination:**
    *   Buttons:  Similar to secondary buttons, but with smaller padding.
    *   Active Page:  `bg-blue-600`, text: white.

### **2.4 Cards**

*   **Content Cards:**
    *   Background: White
    *   Border Radius: `rounded-md`
    *   Padding: `p-4`
    *   Box Shadow: `shadow`
    *   Content:  Headings, text, images, buttons.
*   **Profile Cards:**
    *   Layout: Circular image, name, role, short bio.
    *   Styling:  Similar to content cards, with appropriate spacing for the profile elements.
*   **Feature Cards:**
    *   Layout: Icon, heading, description, and potentially a button or link.
    *   Styling:  Use the primary and secondary colors for visual hierarchy.

### **2.5 Modals & Overlays**

*   **Dialog Boxes:**
    *   Background: White
    *   Border Radius: `rounded-md`
    *   Box Shadow: `shadow-2xl`
    *   Padding: `p-6`
    *   Header: Title, close button (icon).
    *   Content: Form, message, etc.
    *   Footer: Buttons (e.g., Cancel, Save).
    *   Overlay:  Semi-transparent background (e.g., `bg-gray-900 opacity-50`)
*   **Tooltips:**
    *   Background: `#374151` (Gray-700)
    *   Text: White
    *   Border Radius: `rounded-md`
    *   Padding: `py-1 px-2`
    *   Position:  Relative to the element (use a library for positioning).
*   **Dropdown Menus:**
    *   Background: White
    *   Border: 1px solid `#D1D5DB` (Gray-300)
    *   Border Radius: `rounded-md`
    *   Padding: `py-1 px-2`
    *   Items:  Links with `text-gray-700`, hover: `bg-gray-100`

### **2.6 Feedback Elements**

*   **Toast Notifications:**
    *   Position:  Top right or bottom right.
    *   Background: `#16A34A` (Green-600) for success, `#EF4444` (Red-600) for errors, `#FACC15` (Yellow-400) for warnings.
    *   Text: White
    *   Padding: `p-3`
    *   Border Radius: `rounded-md`
    *   Icon:  Checkmark, warning icon, or error icon.
    *   Duration: 3-5 seconds.
*   **Alert Banners:**
    *   Position:  Top or bottom of the screen.
    *   Background: Similar to toast notifications.
    *   Text:  White.
    *   Padding: `p-3`
    *   Border Radius: `rounded-md`
    *   Icon:  Checkmark, warning icon, or error icon.
*   **Loading States:**
    *   Spinners: Use a simple, clean spinner.
    *   Skeleton Screens:  Use skeleton screens for content loading (e.g., for report loading).
    *   Progress Bars: For long-running tasks.

### **2.7 Data Display**

*   **Tables:**
    *   Headers:  `text-gray-500`, bold
    *   Rows:  `hover:bg-gray-100`
    *   Alternating Row Colors:  `bg-gray-50` (optional)
    *   Padding: `py-2 px-4`
    *   Borders:  `border-b border-gray-200`
*   **Lists:**
    *   Unordered Lists:  Bullet points, consistent spacing.
    *   Ordered Lists:  Numbered, consistent spacing.
*   **Badges:**
    *   Background:  Primary, Success, Warning, or Error colors.
    *   Text: White
    *   Border Radius: `rounded-full`
    *   Padding: `py-1 px-2`
    *   Font Size:  0.75rem (12px)
*   **Progress Bars:**
    *   Background: `#E5E7EB` (Gray-200)
    *   Filled Area:  `bg-blue-600` (Primary)
    *   Height:  8px
    *   Border Radius: `rounded-full`

---

## 3. Layout & Responsive Design

### **3.1 Grid System**

*   **Framework:**  Use Tailwind's built-in grid system.
*   **Columns:**  12-column grid.
*   **Breakpoints:**
    *   `sm`: 640px (for small screens)
    *   `md`: 768px (for medium screens)
    *   `lg`: 1024px (for large screens)
    *   `xl`: 1280px (for extra-large screens)
    *   `2xl`: 1536px (for 2x extra-