# Import necessary libraries
import gspread
from oauth2client.service_account import ServiceAccountCredentials
from datetime import datetime, timedelta

# Function to distribute leads among sales reps
def distribute_leads(sheet_id, lead_data):
    # Setup Google Sheets API
    scope = ["https://spreadsheets.google.com/feeds", "https://www.googleapis.com/auth/drive"]
    creds = ServiceAccountCredentials.from_json_keyfile_name('path_to_credentials.json', scope)
    client = gspread.authorize(creds)
    
    # Open the Google Sheet
    sheet = client.open_by_key(sheet_id).sheet1
    
    # Get sales reps and their availability
    sales_reps = sheet.get_all_records()
    
    # Find the next available sales rep
    available_rep = None
    for rep in sales_reps:
        if rep['Availability'] == 'Available':
            available_rep = rep['Name']
            break
    
    # Update the lead with the assigned sales rep
    if available_rep:
        lead_data['Assigned Sales Rep'] = available_rep
        sheet.append_row(list(lead_data.values()))

# Function to extract keywords from comments
def extract_keywords(comments, predefined_keywords):
    keywords_found = [keyword for keyword in predefined_keywords if keyword in comments]
    return keywords_found

# Function to schedule follow-up based on urgency
def schedule_follow_up(urgency):
    today = datetime.now()
    if urgency == 'Immediate':
        follow_up_date = today
    elif urgency == 'Short-term':
        follow_up_date = today + timedelta(days=2)
    elif urgency == 'Medium-term':
        follow_up_date = today + timedelta(weeks=1)
    elif urgency == 'Long-term':
        follow_up_date = today + timedelta(weeks=4)
    
    return follow_up_date

# Example usage
lead_data = {
    'Comments': 'This is an urgent request for a demo.',
    'Urgency': 'Immediate'
}

# Predefined keywords
predefined_keywords = ['urgent', 'budget', 'demo']

# Extract keywords
keywords = extract_keywords(lead_data['Comments'], predefined_keywords)
lead_data['Lead Category'] = ', '.join(keywords)

# Distribute leads
distribute_leads('your_google_sheet_id', lead_data)

# Schedule follow-up
follow_up_date = schedule_follow_up(lead_data['Urgency'])
print(f"Follow-up scheduled for: {follow_up_date}")
