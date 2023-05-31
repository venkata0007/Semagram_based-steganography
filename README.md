# Hiding Image in text file
# Importing libraries and necessaries

Import  required libraries to open/access docx file and to access pixels in image PIL library and mount the ipynb to drive  to access files in the drive(Here files are in/MyDrive/steganography folder)/we can give inputs directly in vscode itself


# steps in hiding the image
1. Observe the size of image and resize if required according to the font and font size such that image fits in word file in width
```python
img = Image.open("/content/drive/MyDrive/steganography/kohli2.jpg")
#these are to be set accordingly
new_height = 500 
new_width = 600
new_size = (new_width,new_height)
resized_img = img.resize(new_size)
resized_img.save("/content/drive/MyDrive/steganography/resized.jpg") #saving the resized image
```
2. Divide the text file into chunks of text with size of chunk equal to width of the image and store it in an array (text_chunks)
```python
from docx import Document
document = Document('/content/drive/MyDrive/steganography/original.docx')
text_list = ''
# Iterate over each paragraph in the document
for paragraph in document.paragraphs:
    # Append the paragraph's text to the list
    text_list+= paragraph.text
# Split the text into chunks of width = image_width characters including spaces
text_chunks = []
chunk = ''
for char in text_list:
    if len(chunk) < new_width:
        chunk += char
    else:
        text_chunks.append(chunk)
        chunk = char
text_chunks.append(chunk)
```

3. Store pixel colours of the image into an array

```python
img = Image.open("/content/drive/MyDrive/steganography/resized.jpg")
pixels = img.load()
arr = np.array(img)
```
4. Iterate trough the array of pixels and text_chunks and assign the colour of each pixel to respective character in the chunk, we are mapping the pixel colours to text characters and dump the coloured paragraphs into docx file
```python
# Create a new document
document = Document()
# Iterate over each chunk of text
for i in range(new_height):
    chunk = text_chunks[i]
    # Add a paragraph to the document with the colored text
    paragraph = document.add_paragraph()
     # Set the spacing before and after the paragraph to zero
    paragraph_format = paragraph.paragraph_format
    paragraph_format.space_before = 0
    paragraph_format.space_after = 0
    # Iterate over each character in the text chunk, adding colored text to the document
    for j, char in enumerate(chunk):
        run = paragraph.add_run(char)
        font = run.font
        font.name = 'Courier New'
        font.size = 1
        font.color.rgb = RGBColor(*pixels[j,i])
# Save the document to a file
document.save('/content/drive/MyDrive/steganography/colored_text.docx')

You can observe the coloured text in the docx file in word file keeping font_size = 1 and font = Courier New
[source image]("https://drive.google.com/file/d/1kIFg-e7GddMdL4tverpcvOUEVrUhF8Yf/view?usp=share_link") + 
[docx]("https://docs.google.com/document/d/1oQzVQX1CTN8cnbxK-66f1K-wPlYDjpna/edit usp=share_link&ouid=100409726951559877182&rtpof=true&sd=true") 
== [docx file with image coloured text]("https://docs.google.com/document/d/1--Ee0dJ9xZljdRWlAMmg1CKMYLeWrDj7/edit?usp=share_link&ouid=100409726951559877182&rtpof=true&sd=true")
