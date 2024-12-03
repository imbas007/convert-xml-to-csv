import argparse
import xml.etree.ElementTree as ET
import pandas as pd
import re
import os

def clean_html(text):
    if text:
        # Remove HTML tags such as <p>, <ul>, <li>
        return re.sub(r'<.*?>', '', text)
    return text

def convert_xml_to_csv(input_file):
    # Parse the XML file
    tree = ET.parse(input_file)
    root = tree.getroot()

    # Extract relevant data into a list of dictionaries
    data = []
    for issue in root.findall('issue'):
        issue_data = {
            "SerialNumber": issue.find('serialNumber').text if issue.find('serialNumber') is not None else None,
            "Type": issue.find('type').text if issue.find('type') is not None else None,
            "Name": issue.find('name').text if issue.find('name') is not None else None,
            "Host": issue.find('host').text if issue.find('host') is not None else None,
            "Path": issue.find('path').text if issue.find('path') is not None else None,
            "Severity": issue.find('severity').text if issue.find('severity') is not None else None,
            "Confidence": issue.find('confidence').text if issue.find('confidence') is not None else None,
            "IssueBackground": clean_html(issue.find('issueBackground').text) if issue.find('issueBackground') is not None else None,
            "RemediationBackground": clean_html(issue.find('remediationBackground').text) if issue.find('remediationBackground') is not None else None,
            "References": clean_html(issue.find('references').text) if issue.find('references') is not None else None
        }
        data.append(issue_data)

    # Convert to a pandas DataFrame
    df = pd.DataFrame(data)

    # Generate the output CSV filename
    base_name = os.path.splitext(input_file)[0]
    output_file = f"{base_name}.csv"

    # Save the DataFrame to a CSV file
    df.to_csv(output_file, index=False)
    print(f"CSV file saved to {output_file}")

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Convert XML to CSV.")
    parser.add_argument("-f", "--file", required=True, help="Input XML file")

    args = parser.parse_args()

    convert_xml_to_csv(args.file)
