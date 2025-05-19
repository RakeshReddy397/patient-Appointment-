# patient-Appointment- This is Link of data cleaning https://colab.research.google.com/drive/1egMk-Y9z1Vr_NEnWWQ44Btcqysdk-8jq?usp=sharing
# Step 1: Upload your file
from google.colab import files
uploaded = files.upload()  # Choose 'cleaned_dataset_final.csv' when prompted

# Step 2: Import required libraries
import pandas as pd

# Step 3: Check uploaded filename
filename = list(uploaded.keys())[0]
print(f"✅ Uploaded file: {filename}")

# Step 4: Load the dataset
try:
    df = pd.read_csv(filename)
    print("✅ Dataset loaded successfully!")
except Exception as e:
    print("❌ Error reading the file:", e)

# Step 5: Display basic info
print("\n--- Dataset Info ---")
print(df.info())
print("\n--- First 5 Rows ---")
print(df.head())

# Step 6: Drop duplicates
df.drop_duplicates(inplace=True)

# Step 7: Remove rows with negative age
df = df[df['age'] >= 0]

# Step 8: Convert date columns to datetime format
df['scheduledday'] = pd.to_datetime(df['scheduledday'], errors='coerce')
df['appointmentday'] = pd.to_datetime(df['appointmentday'], errors='coerce')

# Step 9: Standardize text fields
df['gender'] = df['gender'].str.strip().str.upper()
df['neighbourhood'] = df['neighbourhood'].str.strip().str.title()
df['appointment_weekday'] = df['appointment_weekday'].str.strip().str.capitalize()

# Step 10: Rename incorrect column (if exists)
if 'handcap' in df.columns:
    df.rename(columns={'handcap': 'handicap'}, inplace=True)

# Step 11: Check for missing values
print("\n--- Missing Values Per Column ---")
print(df.isnull().sum())

# Step 12: Save cleaned data to new CSV
cleaned_filename = 'cleaned_data_final_ready.csv'
df.to_csv(cleaned_filename, index=False)
print(f"\n✅ Cleaned data saved as '{cleaned_filename}'")

# Step 13: Download the cleaned file
files.download(cleaned_filename)
