# Markdown Viewer

This project is a Vue-based Markdown Viewer that allows users to upload a `.md` file and view its contents with a dynamic Table of Contents (TOC). The TOC is automatically generated from the headings in the Markdown file, and clicking on the TOC entries scrolls the page to the corresponding section. Additionally, it includes a "Back to Top" button for easy navigation.

## Features

- **File Upload**: Upload a `.md` file from your local system and view its contents in the viewer.
- **Dynamic TOC**: Automatically generates a Table of Contents based on the headings in the Markdown file. Clickable TOC links scroll the page to the respective section.
- **Sticky Sidebar**: The sidebar remains fixed as you scroll through the content, making navigation easy.
- **Scroll to Top**: A "Back to Top" button appears when you scroll down, providing quick access to the top of the page.
- **Responsive Design**: The layout is responsive, with a clean, minimalist style.

## Demo

You can see a working demo of the project here: [Demo Link (...)]()

## Project Structure

The project consists of the following key components:

- **Markdown Viewer Component**: Displays the parsed Markdown content.
- **Table of Contents Component**: Shows the dynamically generated TOC and highlights the current section as you scroll.
- **File Upload**: Uploads the Markdown file and displays the parsed content.

## Installation

1. Clone this repository:

   ```bash
   git clone https://github.com/jefsky/markdown-viewer.git
   ```

2. Navigate to the project directory:

   ```bash
   cd markdown-viewer
   ```

3. Install dependencies:

   ```bash
   npm install
   ```

4. Start the development server:

   ```bash
   npm run serve
   ```

   The app should now be running at `http://localhost:8080`.

## Usage

1. Click on the "Choose File" button in the sidebar to upload a Markdown file.
2. The file will be parsed, and the content will be displayed on the right side of the page.
3. The sidebar will show a Table of Contents generated from the headings (`h1`, `h2`, etc.) in the Markdown file. Clicking on a TOC item will scroll the page to the respective section.
4. A "Back to Top" button will appear as you scroll down, allowing you to return to the top of the page with a single click.

## Code Structure

- **`markdown-container`**: The main container that holds both the sidebar (TOC and file upload button) and the Markdown content.
- **`generateHtml()`**: Uses the `marked` library to parse the Markdown content and extract the headings for the TOC.
- **`scrollTo()`**: Smooth scrolling to the selected heading.
- **`IntersectionObserver`**: Dynamically highlights the TOC item as the user scrolls through the document.

## Customization

### Styling

The project uses basic CSS for styling, which can be customized in the `style` section of the component. Some of the key elements that can be adjusted include:

- Sidebar width and background color
- Button styles for "Choose File" and "Back to Top"
- Font sizes and spacing for TOC items
- Box shadows and border-radius for a clean UI

### Extending the Viewer

You can extend the functionality of the viewer by adding more features like:

- **Markdown Editor**: Allow users to edit the Markdown directly within the application.
- **Themes**: Provide light and dark mode themes for the viewer.
- **Additional File Types**: Support for more file types like `.txt`, `.json`, etc.

## Dependencies

- [Vue.js](https://vuejs.org/): JavaScript framework for building the UI.
- [Marked.js](https://marked.js.org/): A Markdown parser and compiler.
- [VueScrollTo](https://www.npmjs.com/package/vue-scrollto): For smooth scrolling to different sections.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

## Contributing

Feel free to fork this repository and submit pull requests. For major changes, please open an issue first to discuss what you would like to change.

---

Thank you for checking out this project! If you like it, feel free to give it a star 🌟 on [GitHub](https://github.com/jefsky/markdown-viewer).
