import tkinter as tk
from tkinter import filedialog
import cv2
import numpy as np
from PIL import Image, ImageTk

# Function to perform brightness and contrast adjustment
def adjust_brightness_contrast(image, brightness, contrast):
    adjusted = np.int16(image)
    adjusted = adjusted * (contrast / 127 + 1) - contrast + brightness
    adjusted = np.clip(adjusted, 0, 255)
    adjusted = np.uint8(adjusted)
    return adjusted

# Function to perform noise reduction (e.g., Gaussian blur)
def reduce_noise(image):
    blurred = cv2.GaussianBlur(image, (5, 5), 0)
    return blurred

# Function to open a file dialog and load image
def load_image():
    file_path = filedialog.askopenfilename(filetypes=[("JPEG files", "*.jpg;*.jpeg")])
    if file_path:
        original_image = cv2.imread(file_path)
        original_image_resized = cv2.resize(original_image, (512, 512))

        # Display original image
        original_image_rgb = cv2.cvtColor(original_image_resized, cv2.COLOR_BGR2RGB)
        original_image_pil = Image.fromarray(original_image_rgb)
        original_image_tk = ImageTk.PhotoImage(original_image_pil)
        label_original.config(image=original_image_tk)
        label_original.image = original_image_tk

        processed_image = original_image.copy()  # Make a copy for processing

        # Example: Adjust brightness and contrast
        processed_image = adjust_brightness_contrast(processed_image, brightness_value, contrast_value)

        # Example: Reduce noise
        processed_image = reduce_noise(processed_image)

        # Resize the processed image to 512x512
        processed_image = cv2.resize(processed_image, (512, 512))

        # Convert processed image to RGB format for displaying with Tkinter
        processed_image_rgb = cv2.cvtColor(processed_image, cv2.COLOR_BGR2RGB)
        processed_image_pil = Image.fromarray(processed_image_rgb)
        processed_image_tk = ImageTk.PhotoImage(processed_image_pil)

        # Display processed image
        label_processed.config(image=processed_image_tk)
        label_processed.image = processed_image_tk

        # Update the text labels
        label_text_original.config(text="Input Image")
        label_text_processed.config(text="Enhanced Image")

# Example GUI setup
root = tk.Tk()
root.title("Low-Light Image Enhancer")
root.config(bg="orange")

# GUI components
title_label = tk.Label(root, text="VARDHAMAN COLLEGE OF ENGINEERING", font=("Helvetica", 30, "bold"), fg="red", bg='orange')
title_label.pack(pady=10)

title_label = tk.Label(root, text="MINI PROJECT OUTPUT", font=("Helvetica", 25, "bold"), fg="navy blue", bg='orange')
title_label.pack(pady=10)

button_load = tk.Button(root, text="Upload Image", command=load_image,font=("Helvetica", 20), width=20, height=2,bg="yellow")
button_load.pack(pady=10)

frame_images = tk.Frame(root, bg="light yellow")
frame_images.pack(padx=10, pady=10)

label_original = tk.Label(frame_images, bg="light grey")
label_original.grid(row=0, column=0, padx=10, pady=10)

label_processed = tk.Label(frame_images, bg="light grey")
label_processed.grid(row=0, column=1, padx=10, pady=10)

label_text_original = tk.Label(frame_images, text="", bg="light grey")
label_text_original.grid(row=1, column=0, pady=10)

label_text_processed = tk.Label(frame_images, text="", bg="light grey")
label_text_processed.grid(row=1, column=1, pady=10)

# Example variables for brightness and contrast adjustments
brightness_value = 170
contrast_value = 90

# Run the GUI
root.mainloop()
