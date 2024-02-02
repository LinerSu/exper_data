import sys
import pandas as pd

# Load the CSV file
df = pd.read_csv(sys.argv[1])  # Replace 'your_file.csv' with your actual file path

# Function to check if a value is a float or one of the allowed strings
def is_time_valid(value):
    if value in ["timeout", "error", "no result"]:
        return True
    try:
        float(value)
        return True
    except ValueError:
        return False

# Check if the 'time' column is consistent
df['time_valid'] = df['time'].apply(is_time_valid)

# Check if 'safe', 'error', 'warning' are integers and handle the special case
def check_int_columns(row):
    if row['time'] in ["timeout", "error", "no result"]:
        return (row['safe'] == 0) and (row['error'] == 0) and (row['warning'] == 0)
    else:
        return isinstance(row['safe'], int) and isinstance(row['error'], int) and isinstance(row['warning'], int)

# Applying the check
df['int_columns_valid'] = df.apply(check_int_columns, axis=1)

# Checking for inconsistencies
if df['time_valid'].all() and df['int_columns_valid'].all():
    print("The file is consistent.")
else:
    print("Inconsistencies found.")
    if not df['time_valid'].all():
        print("Inconsistent values in 'time' column:", df.loc[~df['time_valid'], 'time'])
    if not df['int_columns_valid'].all():
        print("Inconsistent values in 'safe', 'error', 'warning' columns for given 'time' conditions.")