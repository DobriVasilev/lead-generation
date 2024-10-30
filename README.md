# lead-generation
Find, qualify, and contact domain names.

With the tutorial below you'll be able to find thousands and thousands of business owners in minutes. 

Then make sure they have cash to pay you by scraping their WHOLE website visits analytics, and then you'll scrape ALL of their privately accessible social accounts, email addresses, and phone numbers. 

Lastly, you'll automate everything to send hundreds of personalized emails to all of those leads.

In this tutorial, I'm using 2 APIs, 1 automaton (make.com), and 1 email auto sender (instantly.ai)

All of them come out to $209 per month. If you don't have at least that then:

![you're-broke-broke](https://github.com/user-attachments/assets/7ed137a8-cbc0-4cd8-9bfc-69be8112e4c0)


## №1 Find Domains
Here's what exactly you need to get started.

First, you need to have a list of hundreds of domains already generated. If you don't yet have it then use the API below:

https://zylalabs.com/api-marketplace/finance/scraped+business+data+api/1280


It'll allow you to find local business contacts in a specific radius. I haven't yet created a script that will automatically sort all the info but you can do it yourself. Don't be lazy go ask ChatGPT how to do that.

Alternatively, if you don't want to use this method just go check the AAA campus in TRW and use their scraping method (what I'm doing).




## №2 Qualify Domains


Next step is making sure you're reaching out to businesses that can actually pay you something -> they have traffic coming (thus money flowing).

I'm qualifying leads based on their monthly traffic -> **If for the past 3 months, it has been over 10k they qualify**

The API below scrapes Similar Web to find the data. (If you have $1-$2k/month to pay for more accurate data then use SEMrush)

To start qualifying leads you need to:

**1. Have bought this API:**
https://zylalabs.com/api-marketplace/ip%20&%20domain/Web+Traffic+Watcher+API/5165/#pricing

![chrome_wLvbh71Z1C](https://github.com/user-attachments/assets/5b614715-8075-490d-8dc3-14fb1eedaa46)



**2. Create a txt file in an easy to find folder**

This is your script which will automatically qualify hundreds of domains in seconds. 

(script = text file)

You want to create it in a folder that is SUPER simple to find (e.g. D:/Documents/)

Why?

Because you'll have to use PowerShell/CMD/CLI to access it => **USE CODE 😱**

After creating the txt file you want to paste the code below in it:

```
import requests
import time
import csv
from datetime import datetime

# Helper function to subtract months
def subtract_months(dt, months):
    year = dt.year
    month = dt.month - months
    while month <= 0:
        month += 12
        year -= 1
    return dt.replace(year=year, month=month, day=1)

# Your API Access Key
api_key = "API_KEY"
url = "https://zylalabs.com/api/5165/web+traffic+watcher+api/6596/site+traffic+stats"

# Path to the text file containing the list of domains
file_path = "C:\\Users\\PC\\Desktop\\domain.txt"
# Path to the output CSV file
csv_file_path = "C:\\Users\\PC\\Desktop\\results.csv"

# Prepare CSV file and write header
with open(csv_file_path, mode='w', newline='') as csv_file:
    writer = csv.writer(csv_file)
    # Write the header row
    writer.writerow(["Domain", "Traffic 3 Months Ago", "Traffic 2 Months Ago", "Traffic 1 Month Ago"])

# Read domains from file
with open(file_path, "r") as file:
    domains = [line.strip() for line in file if line.strip()]

# Calculate the dates for the last 3 months
months = []
for i in range(3, 0, -1):  # from 3 to 1
    date = subtract_months(datetime.today(), i)
    months.append(date.strftime("%Y-%m-01"))

# For debugging: print the months being used
print("Months being used:", months)

# Loop through each domain and process it
for domain in domains:
    print(f"Analyzing domain: {domain}")
    
    # Set up headers with API key for authentication
    headers = {
        "Authorization": f"Bearer {api_key}"
    }
    
    # Initialize retry parameters
    max_retries = 3
    retries = 0
    success = False
    
    while retries < max_retries and not success:
        # Request data for each domain
        response = requests.get(url, headers=headers, params={"domain": domain})
        
        if response.status_code == 200:
            data = response.json()
            # Extract only EstimatedMonthlyVisits for each domain
            if "EstimatedMonthlyVisits" in data:
                traffic_3_months = data["EstimatedMonthlyVisits"].get(months[0], "N/A")
                traffic_2_months = data["EstimatedMonthlyVisits"].get(months[1], "N/A")
                traffic_1_month = data["EstimatedMonthlyVisits"].get(months[2], "N/A")
                
                # Print the result
                print(f"{domain} - Traffic 3 Months Ago: {traffic_3_months}, 2 Months Ago: {traffic_2_months}, 1 Month Ago: {traffic_1_month}")
                
                # Save to CSV
                with open(csv_file_path, mode='a', newline='') as csv_file:
                    writer = csv.writer(csv_file)
                    writer.writerow([domain, traffic_3_months, traffic_2_months, traffic_1_month])
            else:
                print(f"No traffic data for {domain}")
            success = True  # Mark as successful to exit the retry loop
        elif response.status_code == 503:
            retries += 1
            wait_time = 10 + 5 * retries  # Increase wait time with each retry
            print(f"Received 503 error for {domain}. Waiting for {wait_time} seconds before retrying... (Retry {retries}/{max_retries})")
            time.sleep(wait_time)
        else:
            print(f"Error fetching data for {domain}: {response.status_code}")
            success = True  # Exit loop even if there's an error other than 503
        
        # Optional: Pause between requests to avoid rate limiting
        if success:
            time.sleep(1)
    
    if not success:
        print(f"Failed to fetch data for {domain} after {max_retries} retries.")
    
print(f"Data has been saved to {csv_file_path}")

```


Pasted it? Good.

Now find the line where it says: `api_key = "API_KEY"`

You want to change `API_KEY` to your actual API key in ZYLA's website found right above the API. (**don't remove the quotes ""**)

(if you face any errors also check if the `url` variable slightly below is correct)

Next you want to modify these 2 lines:

```
# Path to the text file containing the list of domains
file_path = "C:\\Users\\PC\\Desktop\\domain.txt"
# Path to the output CSV file
csv_file_path = "C:\\Users\\PC\\Desktop\\results.csv"
```

Here `file_path` is the txt file that should contain the domains of your leads.

If you're on Windows you can just create a text file on your desktop that is called `domain.txt` and fill it with your domains (whenever you want to qualify a new batch you'll need to edit the content of this file)

If you're on Mac again create the same `domain.txt` file on your desktop and change the info below to match your exact username:

```
# Path to the text file containing the list of domains
file_path = "/Users/YourUsername/Desktop/domain.txt"
# Path to the output CSV file
csv_file_path = "/Users/YourUsername/Desktop/results.csv"
```

(keep in mind this script will create a CSV file on your desktop which will contain all the leads & their monthly traffic)


Now before you save this text file -> **STOP**

At the end your machine will prompt you to put a .txt prefix by default. But you shouldn't.

**Instead put a `.py` prefix.**

So your name of the text file might look something like: `qualify_leads.py`


### Time to run it

On Windows search for **Power Shell**

Open the blue monster.

On MacOS search for **Terminal**

Open the black monster.



Now paste this command in the terminal:

```
pip install requests
```

It'll install the necessary stuff.



Then locate your folder where you have your script.

To do that just write `cd` followed by the name of that folder. e.g.:

```
cd D

cd Documents
```
Now you're in `D:\Documents\`



Now just run the script:

```
python script.py
```

Change `script` to the name of your script.


Next, check your desktop. You'll find a new csv containing the stats of your qualified leads.

Now upload that in [Google Sheets](https://docs.google.com/spreadsheets/)

Then go to:

![chrome_1E6heKJZo9](https://github.com/user-attachments/assets/f86d2410-6a9c-48b4-b4f9-ddfb25c8328f)



Delete whatever text there is:

![chrome_wi9n7MXOPf](https://github.com/user-attachments/assets/8e6493d3-fa76-4bc2-8e60-f5db58971a46)


And instead, paste this:

```
function deleteRowsBelowThreshold() {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var lastRow = sheet.getLastRow();
    var threshold = 1000;

    // Loop through each row from the last row to the first
    for (var i = lastRow; i >= 2; i--) { // Start at row 2 to avoid the header
        var valueB = sheet.getRange(i, 2).getValue();
        var valueC = sheet.getRange(i, 3).getValue();
        var valueD = sheet.getRange(i, 4).getValue();

        if (valueB < threshold && valueC < threshold && valueD < threshold) {
            sheet.deleteRow(i);
        }
    }
}
```
This will automatically delete all the leads that have under 10k traffic.


Alternatively, you can just use this `qualify_leads.py` script to only save leads that qualify (again change `API_KEY` and `domain.txt` and/or `results.csv` location):

```
import requests
import time
import csv
from datetime import datetime

# Helper function to subtract months
def subtract_months(dt, months):
    year = dt.year
    month = dt.month - months
    while month <= 0:
        month += 12
        year -= 1
    return dt.replace(year=year, month=month, day=1)

# Your API Access Key
api_key = "API_KEY"
url = "https://zylalabs.com/api/5165/web+traffic+watcher+api/6596/site+traffic+stats"

# Path to the text file containing the list of domains
file_path = "C:\\Users\\PC\\Desktop\\domain.txt"
# Path to the output CSV file
csv_file_path = "C:\\Users\\PC\\Desktop\\results.csv"

# Prepare CSV file and write header
with open(csv_file_path, mode='w', newline='') as csv_file:
    writer = csv.writer(csv_file)
    # Write the header row
    writer.writerow(["Domain", "Traffic 3 Months Ago", "Traffic 2 Months Ago", "Traffic 1 Month Ago"])

# Read domains from file
with open(file_path, "r") as file:
    domains = [line.strip() for line in file if line.strip()]

# Calculate the dates for the last 3 months
months = []
for i in range(3, 0, -1):  # from 3 to 1
    date = subtract_months(datetime.today(), i)
    months.append(date.strftime("%Y-%m-01"))

# For debugging: print the months being used
print("Months being used:", months)

# Loop through each domain and process it
for domain in domains:
    print(f"Analyzing domain: {domain}")
    
    # Set up headers with API key for authentication
    headers = {
        "Authorization": f"Bearer {api_key}"
    }
    
    # Initialize retry parameters
    max_retries = 3
    retries = 0
    success = False
    
    while retries < max_retries and not success:
        # Request data for each domain
        response = requests.get(url, headers=headers, params={"domain": domain})
        
        if response.status_code == 200:
            data = response.json()
            # Extract only EstimatedMonthlyVisits for each domain
            if "EstimatedMonthlyVisits" in data:
                traffic_3_months = data["EstimatedMonthlyVisits"].get(months[0], "N/A")
                traffic_2_months = data["EstimatedMonthlyVisits"].get(months[1], "N/A")
                traffic_1_month = data["EstimatedMonthlyVisits"].get(months[2], "N/A")
                
                # Check if all traffic values are over 10000 before saving
                if all(isinstance(traffic, int) and traffic > 10000 for traffic in [traffic_3_months, traffic_2_months, traffic_1_month]):
                    # Print the result
                    print(f"{domain} - Traffic 3 Months Ago: {traffic_3_months}, 2 Months Ago: {traffic_2_months}, 1 Month Ago: {traffic_1_month}")
                    
                    # Save to CSV
                    with open(csv_file_path, mode='a', newline='') as csv_file:
                        writer = csv.writer(csv_file)
                        writer.writerow([domain, traffic_3_months, traffic_2_months, traffic_1_month])
                else:
                    print(f"Skipping {domain} due to low traffic in one or more months.")
            else:
                print(f"No traffic data for {domain}")
            success = True  # Mark as successful to exit the retry loop
        elif response.status_code == 503:
            retries += 1
            wait_time = 10 + 5 * retries  # Increase wait time with each retry
            print(f"Received 503 error for {domain}. Waiting for {wait_time} seconds before retrying... (Retry {retries}/{max_retries})")
            time.sleep(wait_time)
        else:
            print(f"Error fetching data for {domain}: {response.status_code}")
            success = True  # Exit loop even if there's an error other than 503
        
        # Optional: Pause between requests to avoid rate limiting
        if success:
            time.sleep(1)
    
    if not success:
        print(f"Failed to fetch data for {domain} after {max_retries} retries.")
    
print(f"Data has been saved to {csv_file_path}")

```













## №3 Find Phone Numbers Of Those Qualified Leads.

Now the process is quite the same.

You'll use this API for this:
https://zylalabs.com/api-marketplace/data/website+contacts+scraper+api/2396

Create another script that you want to save as `phone.py` in an easy-to-access folder.

There paste this:

```
import requests
import time
import csv

# Your API Access Key
api_key = "API_KEY"
url = "https://zylalabs.com/api/2396/website+contacts+scr"

# Path to the text file containing the list of domains
file_path = "C:\\Users\\PC\\Desktop\\domain.txt"
# Path to the output CSV file
csv_file_path = "C:\\Users\\PC\\Desktop\\phone.csv"

# Read domains from file
with open(file_path, "r") as file:
    domains = [line.strip() for line in file if line.strip()]

all_data = []

# Loop through each domain and process
for domain in domains:
    print(f"Processing domain: {domain}")
    
    # Set up headers with API key for authentication
    headers = {
        "Authorization": f"Bearer {api_key}"
    }
    params = {"query": domain}
    
    # Initialize retry parameters
    max_retries = 3
    retries = 0
    success = False
    
    while retries < max_retries and not success:
        # Request data for each domain
        response = requests.get(url, headers=headers, params=params)
        
        if response.status_code == :
            data = response.json()
            # Extract data
            if "data" in data and len(data["data"]) > 0:
                item = data["data"][0]
                
                # Extract emails
                emails_list = [email['value'] for email in item.get('emails', [])]
                
                # Extract phone numbers
                phone_numbers_list = [phone['value'] for phone in item.get('phone_numbers', [])]
                
                # Limit phone numbers to 3, including at least one starting with '1' if available
                if len(phone_numbers_list):
                    numbers_starting_with_1 = [num for num in phone_numbers_list if num.startswith('1')]
                    other_numbers = [num for num in phone_numbers_list if not num.startswith('1')]
                    
                    final_numbers = []
                    if numbers_starting_with_1:
:
                        # Include first number starting with '1'
                        final_numbers.append(numbers_starting_with_1[0])
                        # Fill remaining slots with other numbers
                        remaining_slots = 2
                        final_numbers.extend(other_numbers[:remaining_slots])
                        # If still less than 3 numbers, add more numbers starting with '1'
                        if len(final_numbers):
:
                            additional_numbers_needed:
                            final_numbers.extend(additional_numbers[:(3 - len(final_numbers))])
                    else:
                        # No numbers starting with '1', take first 3 numbers
                        final_numbers = phone_numbers_list[:3]
                    
                    phone_numbers_list = final_numbers
                else:
                    phone_numbers_list = phone_numbers_list  # Less than or equal to 3 numbers
                
                # Extract social profiles
                socials = {
                    'Facebook': item,
                    'Instagram': item.get('instagram,
                    'TikTok': item.get('tiktok') or '',
                    'Twitter': item.get('twitter') or '',
                    'LinkedIn': item.get('linkedin') or '',
                    'GitHub': item.get('github') or '',
                    'You,
                    'Pinterest': item.get('pinterest') or ''
                }
                
                domain_data = {
                    'Domain': domain,
                    'Emails': emails_list,
                    'Phone Numbers': phone_numbers_list,
                    **socials
                }
                
                all_data.append(domain_data)
                
                print(f"Data for {domain} collected.")
            else:
                print(f"No data found for {domain}")
                all_data.append({'Domain': domain})
            success = True  # Mark as successful to exit the retry loop
        elif response.status_code == 503:
            retries += 1
            wait_time = 10 + 5 * retries  # Increase wait time with each retry
            print(f"Received 503 error for {domain}. Waiting for {wait_time} seconds before retrying... (Retry {retries}/{max_retries})")
            time.sleep(wait_time)
        else:
            print(f"Error fetching data for {domain}: {response.status_code}")
            all_data.append({'Domain': domain})
            success = True  # Exit loop even if there's an error other than 503
        
        # Optional: Pause between requests to avoid rate limiting
        if success:
            time.sleep(1)
    
    if not success:
        print(f"Failed to fetch data for {domain} after {max_retries} retries.")
        all_data.append({'Domain': domain})

# Determine the maximum number of emails and phone numbers (limit phone numbers to 3)
max_emails = max(len(data.get('Emails',) for data in all_data)
max_phones = min(max(len(data, 3) for data in all_data)

# Prepare CSV headers
headers = ['Domain']
headers += [f'Email #{i+1}' for i in range(max_emails)]
headers += [f'Phone Number #{i+1}' for i in range(max_phones)]
headers += ['Facebook', 'Instagram', 'TikTok', 'Twitter', 'LinkedIn', 'GitHub', 'YouTube', 'Pinterest']

# Write to CSV
with open(csv_file_path, mode='w', newline='', encoding='utf-8') as csv_file:
    writer = csv.DictWriter(csv_file, fieldnames=headers)
    writer.writeheader()
    
    for data in all_data:
        row = {'Domain': data.get('Domain', '')}
        # Add emails
        emails = data.get('Emails', [])
        for i in range(max_emails):
            key = f'Email #{i+1}'
            row[key maybe) else ''
        # Add phone numbers
        phones = data.get('Phone Numbers', [])
        for i in range(max_phones):
            key = f'Phone Number #{i+1}'
            row = phones[i maybe) else ''
        # Add socials
        for social in ['Facebook', 'Instagram', 'TikTok', 'Twitter', 'LinkedIn', 'GitHub', 'YouTube', 'Pinterest']:
            row[s data.get(s , '')
        writer.writerow(row)

print(f"Data has been saved to {csv_file_path}")
```

Again change the `API_KEY` and the `domain.txt` and/or `phone.csv` location.


Then update the `domain.txt` file to only include the leads that currently qualify (those that you just scanned).


Now just run it with:

```
cd [the folder where your script is]

python phone.py
```



# YOU'RE DONE

Now you've qualified and found the contacts of thousands 10,000 leads (the amount you're allowed with the $50 plan).



All that's left is to follow the email automation course in the AAA campus to create the make.com flow (System 1 & System 3) and automate it with instantly.ai to be able to send hundreds of emails per day.


****_BYE_****

****P.S.**** You can use the below JSON to upload it in make.com then change all the info to match your exact APIs and other info. 

****P.P.S.**** You'll notice that I also give GPT info from the company's X account as the contact scraper above gives you that info

```

{
    "name": "Integration Google Sheets",
    "flow": [
        {
            "id": 1,
            "module": "google-sheets:filterRows",
            "version": 2,
            "parameters": {
                "__IMTCONN__": "API_CONNECTION_ID"
            },
            "mapper": {
                "from": "drive",
                "limit": "27",
                "sheetId": "SHEET_ID",
                "sortOrder": "asc",
                "spreadsheetId": "SPREADSHEET_ID",
                "tableFirstRow": "A1:Z1",
                "includesHeaders": true,
                "valueRenderOption": "FORMATTED_VALUE",
                "dateTimeRenderOption": "FORMATTED_STRING"
            },
            "metadata": {
                "designer": {
                    "x": 0,
                    "y": 0
                },
                "restore": {
                    "expect": {
                        "spreadsheetId": {
                            "mode": "chose",
                            "label": "SPREADSHEET_LABEL"
                        }
                    },
                    "parameters": {
                        "__IMTCONN__": {
                            "data": {
                                "scoped": "true",
                                "connection": "google"
                            },
                            "label": "Google Connection"
                        }
                    }
                }
            }
        },
        {
            "id": 10,
            "module": "http:ActionSendData",
            "version": 3,
            "parameters": {
                "handleErrors": true,
                "useNewZLibDeCompress": true
            },
            "mapper": {
                "url": "https://scrapeninja.p.rapidapi.com/scrape",
                "data": "{\n\n\"url\":\"{{2.linkedin}}\"\n\n}",
                "headers": [
                    {
                        "name": "X-Rapidapi-Key",
                        "value": "RAPIDAPI_KEY"
                    },
                    {
                        "name": "X-Rapidapi-Host",
                        "value": "scrapeninja.p.rapidapi.com"
                    }
                ],
                "authPass": "AUTH_PASS",
                "authUser": "AUTH_USER"
            }
        },
        {
            "id": 14,
            "module": "http:ActionSendData",
            "version": 3,
            "parameters": {
                "handleErrors": true,
                "useNewZLibDeCompress": true
            },
            "mapper": {
                "url": "https://api.instantly.ai/api/v1/lead/add",
                "data": "{\n  \"api_key\": \"INSTANTLY_API_KEY\",\n  \"campaign_id\": \"CAMPAIGN_ID\",\n  \"skip_if_in_workspace\": false,\n  \"skip_if_in_campaign\": true,\n  \"leads\": [\n    {\n      \"email\": \"{{2.email}}\",\n      \"first_name\": \"{{2.first_name}}\",\n      \"last_name\": \"{{2.sur_name}}\",\n      \"company_name\": \"{{2.company_name}}\",\n      \"phone\": \"{{2.phone}}\",\n      \"website\": \"{{2.company_domain}}\",\n      \"custom_variables\": {\n        \"icebreaker\": \"{{replace(replace(19.icebreaker; \"\"\"\"; \"'\"); newline; \"\\n\")}}\"\n      }\n    }\n  ]\n}"
            }
        }
    ],
    "metadata": {
        "instant": false,
        "version": 1,
        "zone": "eu2.make.com"
    }
}


```







