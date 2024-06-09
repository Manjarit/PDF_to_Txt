import os
import pdfplumber
import pandas as pd

# Define input and output directories
input_folder = 'C:\\Users\\manja\\Desktop\\Marriage\\Input\\'
output_folder = 'C:\\Users\\manja\\Desktop\\Marriage\\Output\\'

# Iterate over each PDF file in the input folder
for filename in os.listdir(input_folder):
    if filename.endswith('.pdf'):
        pdf_path = os.path.join(input_folder, filename)
        
        # Extracting the name without extension for the output text file
        file_name_without_extension = os.path.splitext(filename)[0]
        txt_path = os.path.join(output_folder, file_name_without_extension + '.txt')
        
        # Open the PDF file and extract text
        with pdfplumber.open(pdf_path) as source:
            with open(txt_path, 'w', encoding='utf-8') as extract:
                pg_num = len(source.pages)
                for i in range(pg_num):
                    pg_obj = source.pages[i]
                    txt_obj = pg_obj.extract_text()
                    extract.write(txt_obj)

# Now let's read all text files in the output folder and create a DataFrame
all_data = []
for txt_filename in os.listdir(output_folder):
    if txt_filename.endswith('.txt'):
        txt_filepath = os.path.join(output_folder, txt_filename)
        with open(txt_filepath) as f:
            data = f.readlines()
            all_data.extend(data)

# Create DataFrame from the extracted text
df = pd.DataFrame(all_data)
print(df)
