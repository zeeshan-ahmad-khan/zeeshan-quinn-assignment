# Quinn Assignment: Infinite Scrollable Calendar

A React and TypeScript application that displays a performant, infinitely scrollable calendar with a UI designed to precisely match the provided video specification.

## Live Demo

**The deployed project can be viewed here:** [https://quinn-assignment-zeeshan.netlify.app/](https://quinn-assignment-zeeshan.netlify.app/)

## Features

- **Continuous Infinite Scrolling**: Seamlessly scroll through a single, continuous grid of days.
- **Dynamic Header**: The header always displays the month and year that is most visible in the viewport.
- **Journal Entries**: Displays journal entries from a dataset on the correct dates.
- **Swipable Card UI**: Clicking an entry opens a modal where you can swipe to navigate between entries, view details, and see category tags.
- **Performant**: Uses virtualization and precise scroll adjustments to ensure smooth, lag-free scrolling.
- **Responsive Design**: The layout adapts from a mobile-first view to tablets and desktops.
- **Polished UI**: Features a fixed header and weekday bar, a static footer, in-cell month labels, and a hidden scrollbar for a native-app feel.

## Tech Stack

- **Framework**: React with Vite
- **Language**: TypeScript
- **Date Management**: `date-fns`
- **Styling**: Vanilla CSS

## How to Run Locally

1.  **Clone the repository:**

    ```bash
    git clone <your-repo-url>
    cd <repo-name>
    ```

2.  **Install dependencies:**

    ```bash
    npm install
    ```

3.  **Start the development server:**
    ```bash
    npm run dev
    ```

The application will now be running on `http://localhost:5173` (or the next available port).

## Design Choices & Architecture

The core challenge of this assignment is creating a smooth infinite scroll experience in a continuous grid without performance degradation. The final architecture was chosen to solve this specifically.

- **Continuous Grid Layout**: Instead of rendering separate grids for each month, the application calculates a single, flat array of all visible days. These days are then rendered into one continuous CSS grid inside the main `App` component. This approach creates the seamless "side-by-side" month flow seen in the video.

- **Virtualization**: While the grid is visually continuous, the underlying data is virtualized. Only a "window" of months (e.g., 7) is kept in the state. As the user scrolls, this window slides, and the `allDays` array is recalculated, keeping the number of DOM nodes manageable.

- **Intersection Observer**: This API is used for two key tasks:

  1.  **Infinite Scroll Trigger**: Invisible "sentinel" elements at the top and bottom of the grid trigger functions to load past or future days.
  2.  **Header Updates**: The observer watches individual `DayCell` components to determine which month has the most visible days on screen, allowing the header to update dynamically.

- **Precise Scroll Adjustment**: To solve the common issue of "jitter" when prepending new items to a scrollable list, this project uses a combination of `flushSync` from `react-dom` and direct DOM measurements. This forces React to render new days synchronously, allowing us to calculate and apply the correct scroll offset in a single step, which makes scrolling backward perfectly smooth.
