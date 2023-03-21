# TesseractOCRFixedIssue

Tesseract OCR is a powerful tool for extracting text from images, but its accuracy can be affected by various factors like image quality, text layout, and font type. To improve the text extraction, you can use a combination of image preprocessing techniques and Tesseract configuration options. Here's a Python implementation using the pytesseract wrapper and PIL for image processing:

```import pytesseract
from PIL import Image, ImageEnhance, ImageFilter

# Preprocess image
def preprocess_image(image_path):
    image = Image.open(image_path)
    
    # Convert to grayscale
    image = image.convert('L')
    
    # Apply a mild Gaussian blur to reduce noise
    image = image.filter(ImageFilter.GaussianBlur(1))
    
    # Adjust brightness and contrast
    enhancer = ImageEnhance.Brightness(image)
    image = enhancer.enhance(1.5)
    enhancer = ImageEnhance.Contrast(image)
    image = enhancer.enhance(2.0)
    
    # Binarize image using an adaptive threshold
    image = image.point(lambda x: 0 if x < 128 else 255, '1')
    
    return image

def extract_text(image_path):
    # Preprocess image
    image = preprocess_image(image_path)
    
    # Tesseract configuration
    custom_config = r"--oem 3 --psm 6"
    
    # Extract text from image
    text = pytesseract.image_to_string(image, config=custom_config)
    
    return text

# Example usage
image_path = "path/to/your/image.png"
text = extract_text(image_path)
print(text)```

#This code snippet demonstrates how to preprocess an image before passing it to Tesseract OCR. Preprocessing steps include converting the image to grayscale, applying a Gaussian blur to reduce noise, enhancing brightness and contrast, and binarizing the image using an adaptive threshold.

Additionally, the Tesseract configuration options --oem 3 and --psm 6 are set to use the default OCR engine mode (LSTM) and the automatic page segmentation mode with block orientation detection, respectively. These options can be adjusted based on your specific requirements.

Remember to install the required packages before running the code:
pip install pytesseract
pip install Pillow

Keep in mind that OCR is a challenging task, and the accuracy of Tesseract may still vary depending on the image quality and text properties. To further improve the results, you can experiment with different preprocessing techniques or explore other OCR libraries like CRAFT, EasyOCR, or OCRopus.

