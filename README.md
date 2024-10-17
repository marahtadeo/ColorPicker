# ColorPicker
Design a simple user interface (UI) and implement specific functionality based on the given requirements.
Requirements:
Color List Display: The left side of the UI should display a list of colors fetched from the Colors API.
Colors API Endpoint: https://api.prolook.com/api/colors/prolook
Scrollable Color List: The color list on the left side should be scrollable because it may contain a large number of colors. The page itself should not be scrollable.
Interactive Color Preview: Implement a "Preview" button for each color in the list. When a user clicks the "Preview" button for a specific color, the background color of the right-side box content should update to reflect the selected color.
Box Content Update: Additionally, the information inside the box (text and other relevant data) should also update dynamically based on the selected color.
Visibility and Readability: Ensure that text displayed in the box is clearly visible and readable against the background color.

## Tech Stack
- TypeScript
- Vuetify
- Vite

## How to use
1. Clone the repo
``` bash
git clone https://github.com/marahtadeo/ColorPicker.git
```

2. Install dependencies
``` bash
npm install
```

3. Start the dev server
``` bash
npm run dev
```
