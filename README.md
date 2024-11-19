import pytesseract
from PIL import Image
import openai

# Configure Tesseract path if not in system PATH
pytesseract.pytesseract.tesseract_cmd = r"C:\Program Files\Tesseract-OCR\tesseract.exe"

# OpenAI API Key (replace with your own key)
openai.api_key = "your_openai_api_key"

def extract_text_from_image(image_path):
    """
    Extract text from an image using Tesseract OCR.
    """
    try:
        img = Image.open(image_path)
        text = pytesseract.image_to_string(img)
        return text.strip()
    except Exception as e:
        return f"Error reading image: {e}"

def process_question(question):
    """
    Process a question using OpenAI's GPT model.
    """
    try:
        response = openai.Completion.create(
            engine="text-davinci-003",  # Use GPT-3.5 or similar
            prompt=f"Answer this question: {question}",
            max_tokens=100,
            temperature=0.5
        )
        return response['choices'][0]['text'].strip()
    except Exception as e:
        return f"Error processing question: {e}"

def main(image_path):
    """
    Main function to process an image and answer questions.
    """
    print("Extracting text from image...")
    extracted_text = extract_text_from_image(image_path)
    if not extracted_text:
        print("No text found in the image.")
        return

    print(f"Extracted Text: {extracted_text}")
    print("Processing question...")
    answer = process_question(extracted_text)
    print(f"Answer: {answer}")

# Example usage
if __name__ == "__main__":
    image_path = "path_to_your_image.jpg"  # Replace with the image path
    main(image_path)
