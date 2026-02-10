"""
Cloud infrastructure cost estimator using Streamlit.
A Streamlit application to estimate for AWS EC2 and S3 services.

Author: Rosalind Fowlkes
""" 

import streamlit as st


# Page config 
st.set_page_config (page_title="Cloud Calc" , page_icon="💰" , layout="wide")

st.title(" Cloud infrastructure Estimator 💰 ")
st.markdown("Estimate your monthly cloud spend for basic infrastructure:")

# Create columns
col1, col2 = st.columns(2)

with col1:
    st.header("Service Inputs")
    instances = st.number_input ("Number of Instances", min_value=1, value=1)
    hours = st.number_input ("Hours per month", 0, 730, 730)
    
with col2:
    st.header("Estimated Monthly Total")
    # Simple logic check: $05 per hour per instance 
    total = instances * hours * 0.05
    st.metric (label="Total Cost", value=f"${total:,.2f}", delta="Estimated") 

# Sidebar for Inputs
st.markdown("Estimate your monthly cloud spend for basic infrastructure:")

# Sidebar for Inputs
st.sidebar.header("Configure Resources")

service= st.sidebar.selectbox ("Select Service", ["AWS EC2 (Compute)", "AWS S3 (storage)"])

if service == "AWS EC2 (Compute)":
    st.subheader ("EC2 Instance Estimation")
    instance_type = st.selectbox ("Instance Type", ["t3.micro ($0.0104/hr)", "t3.medium ($0.0416/hr)", "m5.large ($0.096/hr)"])

    hours = st.slider ("Hours per Month", 0, 730, 730)

    #Simple logic 
    rates = {"t3.micro ($0.0104/hr)": 0.0104, "t3.medium ($0.0416/hr)": 0.0416, "m5.large ($0.096/hr)": 0.096}
    cost = rates[instance_type] * hours
    st.metric(label="Estimated Monthly Cost", value=f"${cost:.2f}")

elif service == "AWS S3 (Storage)":
    st.subheader ("S3 Storage Estimation")
    gb_storage = st.number_input("Storage (GB)", min_value=0, value=10)


    #Logic: $0.023 per GB storage, $0.09 per GB transfer
    gb_storage = st.number_input("Storage (GB)", value=100)
    data_transfer = st.number_input("Data Transfer (GB)", value=10)
    cost = (gb_storage * 0.023) + (data_transfer * 0.09)

    # Display Cost
    st.metric( label="Estimated Monthly Cost", value=f"${cost:.2f}")

    st.info("Note: These are estimated prices based on US-East-1 region rates.")





