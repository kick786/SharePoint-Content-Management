import requests

# SharePoint site URL
site_url = "https://your-sharepoint-site-url.com"

# SharePoint credentials
username = "your-username"
password = "your-password"

# Define headers for REST API requests
headers = {
    "Accept": "application/json;odata=verbose",
    "Content-Type": "application/json;odata=verbose"
}

# Authenticate and get access token
auth_url = f"{site_url}/_api/contextinfo"
auth_response = requests.post(auth_url, auth=(username, password), headers=headers)
auth_response.raise_for_status()
access_token = auth_response.json()["d"]["GetContextWebInformation"]["FormDigestValue"]

# Define function to upload file to SharePoint
def upload_file_to_sharepoint(file_path, destination_folder):
    # Read file content
    with open(file_path, "rb") as file:
        file_content = file.read()

    # Define upload URL
    upload_url = f"{site_url}/_api/web/GetFolderByServerRelativeUrl('{destination_folder}')/Files/add(url='{file_path}',overwrite=true)"

    # Set headers with access token for upload request
    upload_headers = {
        "Authorization": f"Bearer {access_token}",
        "Accept": "application/json
