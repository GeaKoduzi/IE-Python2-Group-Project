# IE-Python2-GroupProject

### LIBRARIES IMPORT 
import streamlit as st
import pandas as pd

# STANDARD PAGE CONFIGURATIONS
st.set_page_config(layout='wide')

# SAVE FILE SO YOU CAN EASILY NAVIGATE THROUGH DIFFERENT PAGES
@st.cache_data
def load_data(file):
    return pd.read_csv(file) if file is not None else None

# SIDE BAR

# Set Title
st.sidebar.title('Configurations')
# First Function - Upload File
uploaded_file = st.sidebar.file_uploader('Choose a File to Upload', type=['csv'])
# Second Button - Radio Button to Navigate across Different Pages
selected_page = st.sidebar.radio('Navigation', ['About', 'Home', 'Data Cleaning & Transformation', 'Data Visualizations', 'ML Models'])

# Initialize df as None
df = None

# FIRST PAGE
if selected_page == 'About':
    st.markdown('# About')
    st.caption('To Be Decided')

# SECOND PAGE
if selected_page == 'Home':
    st.markdown('# Home')
    st.caption('To be Decided')

    # Display the data in an expander if a file is uploaded
    if uploaded_file is None:
        st.warning('Upload a File in CSV Format')
    else:
        st.success('File has been successfully loaded')

        with st.expander('Click Here to View File'):
            df = load_data(uploaded_file)
            if df is not None:
                st.dataframe(df)

# THIRD PAGE 
if selected_page == 'Data Cleaning & Transformation':
    if uploaded_file is None:
        st.warning("Upload a CSV file to view this page.")
    else:
        df=load_data(uploaded_file)
        # Create One Variable Representative of Categorical Variables and One of Numerical Variables
        numeric_cols = df.select_dtypes(include=['int64', 'float64'])
        categorical_cols = df.select_dtypes(include=['object'])

        # 1) Create an Expander to Display More Information on Numerical Variables
        with st.expander("Click Here to View more information on the Spread of Numerical Variables"):
            # Calculate and store the statistics for each numeric column
            summary_df = pd.DataFrame()
            summary_df['Min'] = numeric_cols.min()
            summary_df['Max'] = numeric_cols.max()
            summary_df['Median'] = numeric_cols.median()
            summary_df['Mode'] = numeric_cols.mode().iloc[0]  # Get the mode(s) - there can be multiple modes
            summary_df['Stdev'] = numeric_cols.std()
            st.write(summary_df)
            st.write('---')

        # 2) Expand the DataFrame with more functionalities
        if 'date' in df.columns:
            # Convert the 'date' column to date only
            df['date'] = pd.to_datetime(df['date'])

            # Create new columns for year, month, and day
            df['year'] = df['date'].dt.year
            df['month'] = df['date'].dt.month
            df['day'] = df['date'].dt.day

            # Display the updated DataFrame
            st.write("Updated DataFrame:")
            st.dataframe(df.head(5))  # Display the first 5 rows of the DataFrame
        else:
            st.warning("The 'date' column is not present in the DataFrame.")
    
