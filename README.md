# Java Server for Dashboard Builder

### **Usage**
In your terminal:

`mvnw spring-boot:run` 

### **DomController Class**
The `DomController` class is a JAX-RS-based RESTful controller that exposes an endpoint to retrieve visualizations from `.rdash` dashboard files. It processes the dashboards to extract widget information and format it into a JSON response.

#### **Imports**
- **Core Java Libraries**: Used for file handling (`File`), I/O operations (`BufferedReader`, `InputStream`), and handling ZIP files.
- **JSON Libraries**: For JSON parsing and object manipulation.
- **Jakarta RESTful Web Services (JAX-RS)**: For REST API annotations and functionality.

---

### **REST Endpoints**

#### `@Path("/dashboards")`
Defines the base path for all endpoints in this controller.

#### **`/visualizations`**
- **HTTP Method**: `GET`
- **Path**: `/visualizations`
- **Response Format**: `application/json`
- **Description**: Retrieves all visualizations from `.rdash` files stored in the `dashboards` folder.

<img width="504" alt="image" src="https://github.com/user-attachments/assets/243815c3-eae5-4c1b-af01-312508357e1f">

---

### **Method Details**

#### **`getRdashData()`**
- **Purpose**: Main function to process `.rdash` files and extract visualization data.
- **Steps**:
  1. Scans the `dashboards` folder for `.rdash` files.
  2. Reads each `.rdash` file as a ZIP archive to find and extract its `.json` content.
  3. Parses the extracted JSON to retrieve dashboard and widget information.
  4. Formats the data into `VisualizationChartInfo` objects.

---

#### **`extractJsonFromRdash(String filePath)`**
- **Purpose**: Extracts the JSON content from a `.rdash` file, treating it as a ZIP archive.
- **Steps**:
  1. Opens the `.rdash` file as a `ZipFile`.
  2. Finds entries ending with `.json`.
  3. Reads and returns the content of the `.json` entry as a string.

---

#### **`extractTitleFromJson(String jsonContent)`**
- **Purpose**: Extracts the `Title` field from the JSON content of a dashboard.
- **Default Behavior**: Returns `"Untitled"` if no `Title` field exists.

---

#### **`parseWidgetsFromJson(String jsonContent, String dashboardFileName, String dashboardTitle)`**
- **Purpose**: Parses the widgets array in the JSON content and maps it to a list of `VisualizationChartInfo` objects.
- **Steps**:
  1. Checks for the presence of a `Widgets` array in the JSON.
  2. Iterates through the widgets to extract:
     - Widget ID
     - Title
     - Visualization settings (e.g., type, view type, chart type)
  3. Determines the appropriate visualization chart type based on the settings.
  4. Generates the image URL for the chart type.
  5. Constructs and returns a list of `VisualizationChartInfo` objects.

---

#### **`getImageUrl(String input)`**
- **Purpose**: Constructs an image URL for a given visualization chart type.
- **Default Behavior**: Appends `.png` to the chart type name, removing the suffix "Visualization" if present.

---

### **Supporting Classes**
Assumes the existence of the following class:

#### **`VisualizationChartInfo`**
- **Purpose**: Represents information about a single visualization chart.
- **Fields**:
  - `dashboardFileName`: The name of the dashboard file.
  - `dashboardTitle`: The title of the dashboard.
  - `vizId`: The unique ID of the visualization widget.
  - `vizTitle`: The title of the widget.
  - `vizChartType`: The type of the chart (e.g., Line, Bar, Gauge).
  - `vizImageUrl`: URL for the visualization chart image.

---

### **Key Features**
1. **Dynamic Parsing**:
   - Automatically discovers `.rdash` files and extracts their visualization data without hardcoding file paths.
   
2. **Flexible Chart Type Handling**:
   - Supports multiple visualization types with fallbacks for unknown types.
   
3. **Error Handling**:
   - Logs errors when `.rdash` files are missing or malformed JSON is encountered.

4. **Extensibility**:
   - Easily extendable to handle additional visualization settings or customization.

5. **RESTful Design**:
   - Simplifies integration with frontend or other services by providing structured JSON data.

---

This documentation provides a comprehensive overview of the `DomController` class and its functionality for working with `.rdash` dashboard files. If you need further details, such as example responses or additional classes, feel free to ask!
