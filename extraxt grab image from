import os
import pyodbc
import requests

# SQL Server connection details
server = '192.168.0.6\\sqlserver2014'
database = 'ftv_eo'
username = 'varjavand'
password = '123456789'

# Directory to save images
base_directory = r'c:\amin\prj'

# Connect to SQL Server
connection_string = f'DRIVER={{SQL Server}};SERVER={server};DATABASE={database};UID={username};PWD={password}'
connection = pyodbc.connect(connection_string)
cursor = connection.cursor()

# Query to get URLs and dates where id_prj = 36
query = """
    SELECT url_pic, date_upload
    FROM tbl_upload_pictures
    WHERE id_prj = 36 and date_upload =14030613
"""
cursor.execute(query)
rows = cursor.fetchall()

# Process each row
for row in rows:
    url_pic, date_upload = row
    
    # Extract year, month, and day from the date_upload
    year = date_upload[:4]
    month = date_upload[4:6]
    day = date_upload[6:]
    
    # Create directory path based on the date
    directory_path = os.path.join(base_directory, year, month)
    os.makedirs(directory_path, exist_ok=True)
    
    # Extract the filename from the URL
    filename = url_pic.split("/")[-1]
    file_path = os.path.join(directory_path, filename)
    
    # Download the image and save it to the specified location
    try:
        response = requests.get(url_pic)
        response.raise_for_status()  # Raise an error for bad responses (e.g., 404)
        
        with open(file_path, 'wb') as file:
            file.write(response.content)
        
        print(f"Saved: {file_path}")
    except requests.exceptions.RequestException as e:
        print(f"Failed to download {url_pic}: {e}")

# Close the database connection
cursor.close()
connection.close()
