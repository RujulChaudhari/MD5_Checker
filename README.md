# File Checker Program Project

## Overview:

This project is a simple file checker implemented in Python with a graphical user interface (GUI). The program allows users to select a file, check its MD5 hash value, file size, last modified date, and file owner. The results are displayed in a user-friendly interface.

## Key Features:

- **File Selection:** Users can browse and select a file using the GUI.
- **Hash Value Check:** Calculates the MD5 hash value of the selected file.
- **File Size Check:** Displays the size of the selected file in bytes.
- **Last Modified Check:** Shows the last modified date of the selected file.
- **File Owner Check:** Attempts to determine the name of the user who last modified the file.
- **Responsive GUI:** The program's interface adapts to changes in window size.

## Prerequisites:

- Python installed on your computer.
- A text editor, such as [Visual Studio Code](https://code.visualstudio.com/download).

## Step 1: Set Up Your Development Environment

1. **Install Python:**
   - Download and install Python from [python.org](https://www.python.org/downloads/).

2. **Install a Text Editor:**
   - Download and install a text editor, such as [Visual Studio Code](https://code.visualstudio.com/download).

## Step 2: Create the File Checker Program

1. **Open Visual Studio Code:**
   - Open Visual Studio Code on your computer.

2. **Create a New Python Script:**
   - Create a new Python script file (e.g., `file_checker.py`) in Visual Studio Code.

3. **Copy the Code:**
   ```python
   import tkinter as tk
   from tkinter import ttk, filedialog
   import hashlib
   import os
   from datetime import datetime

   class FileCheckerApp:
       def __init__(self, master):
           # Initialize the main application window
           self.master = master
           master.title("File Checker")

           # Create the user interface components
           self.create_widgets()

       def create_widgets(self):
           # Configure the style for buttons
           style = ttk.Style()
           style.configure('TButton', padding=6, relief="flat", background="#ccc")

           # Label to display instructions
           self.label = ttk.Label(self.master, text="Select a file:")
           self.label.grid(row=0, column=0, padx=10, pady=10, sticky="w")

           # Button to browse and select a file
           self.browse_button = ttk.Button(self.master, text="Browse", command=self.browse_file)
           self.browse_button.grid(row=0, column=1, padx=10, pady=10, sticky="ew")

           # Button to initiate file checking
           self.check_button = ttk.Button(self.master, text="Check File", command=self.check_file)
           self.check_button.grid(row=1, column=0, columnspan=2, pady=10, sticky="ew")

           # Make the result_label widget selectable
           self.result_label = tk.Text(self.master, wrap="word", height=10, width=50)
           self.result_label.grid(row=2, column=0, columnspan=2, pady=10, sticky="ew")
           self.result_label.config(state=tk.DISABLED)  # Make it read-only initially

       def browse_file(self):
           # Open a file dialog to select a file and store the file path
           file_path = filedialog.askopenfilename()
           self.file_path = file_path
           # Update the label to show the selected file name
           self.label.config(text=f"Selected File: {os.path.basename(file_path)}")

       def check_file(self):
           # Check if a file is selected
           if hasattr(self, 'file_path'):
               # Calculate hash, size, modification date, and owner of the selected file
               hash_value, file_size, last_modified, file_owner = self.calculate_file_info(self.file_path)

               # Display the results in the result_label as selectable text
               result_text = (
                   f"Hash Value: {hash_value}\n"
                   f"File Size: {file_size} bytes\n"
                   f"Last Modified: {last_modified}\n"
                   f"File Owner: {file_owner}"
               )

               self.result_label.config(state=tk.NORMAL)  # Enable editing state
               self.result_label.delete(1.0, tk.END)  # Clear previous content
               self.result_label.insert(tk.END, result_text)  # Insert new content
               self.result_label.config(state=tk.DISABLED)  # Disable editing state

           else:
               # Display a message if no file is selected
               self.label.config(text="Please select a file first!")

       def calculate_file_info(self, file_path):
           # Calculate MD5 hash, file size, last modified date, and file owner
           hash_md5 = hashlib.md5()
           file_size = os.path.getsize(file_path)
           last_modified_timestamp = os.path.getmtime(file_path)
           last_modified = datetime.fromtimestamp(last_modified_timestamp).strftime('%Y-%m-%d %H:%M:%S')

           try:
               file_owner = os.path.basename(os.path.expanduser(os.path.expandvars(os.path.join('~', file_path))))
           except Exception as e:
               file_owner = "N/A"

           with open(file_path, "rb") as f:
               for chunk in iter(lambda: f.read(4096), b""):
                   hash_md5.update(chunk)

           return hash_md5.hexdigest(), file_size, last_modified, file_owner

   if __name__ == "__main__":
       # Create the main application window
       root = tk.Tk()
       # Instantiate the FileCheckerApp class
       app = FileCheckerApp(root)

       # Configure grid weights for responsiveness
       for i in range(3):
           root.grid_rowconfigure(i, weight=1)
       root.grid_columnconfigure(0, weight=1)
       root.grid_columnconfigure(1, weight=1)

       # Start the application event loop
       root.mainloop()

## Step 3: Install Required Packages

1. **Open Terminal in Visual Studio Code:**
   - Open the integrated terminal in Visual Studio Code.

2. **Install PyInstaller:**
   - Run the following command in the terminal to install PyInstaller, which will help compile the Python script into an executable file.
     ```bash
     pip install pyinstaller
     ```

## Step 4: Compile the Program

1. **Verify Script Works:**
   - Ensure that the Python script works without any errors by running it in Visual Studio Code.

2. **Compile as .exe File:**
   - Run the following command in the terminal to compile the script as a standalone executable (.exe) file.
     ```bash
     pyinstaller --onefile file_checker.py
     ```
   - This will create `build` and `dist` folders in your project directory, and the compiled `.exe` file will be in the `dist` folder.
   - ![](https://i.imgur.com/he9pv80.png)

3. **Run the .exe File:**
   - Locate the compiled `.exe` file in the `dist` folder and run it to check if it's working.

   ```note
   NOTE: Some anti-virus software may flag the executable file. You might need to add the file to your anti-virus exceptions.

4. Enjoy!! :)
