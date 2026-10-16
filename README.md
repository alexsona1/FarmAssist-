import os
import pandas as pd
import streamlit as st
from PIL import Image

# 1. Page Configuration
st.set_page_config(
    page_title="FarmAssist - Digital Farm Manager",
    page_icon="🌱",
    layout="wide",
    initial_sidebar_state="expanded",
)

# 2. Custom Styling & Header
st.title("🌱 FarmAssist: Digital Farm Manager")
st.markdown(
    "Your all-in-one assistant for crop management, livestock tracking, and instant AI disease diagnosis."
)

# 3. Sidebar Navigation
st.sidebar.title("Navigation")
app_mode = st.sidebar.selectbox(
    "Choose a Section",
    [
        "Dashboard",
        "Crop & Garden Manager",
        "AI Pest & Disease Diagnostic",
        "Livestock & Feed Tracker",
        "Offline Sync Status",
    ],
)

# -------------------------------------------------------------
# TAB 1: DASHBOARD
# -------------------------------------------------------------
if app_mode == "Dashboard":
  st.subheader("📊 Farm Overview")

  col1, col2, col3, col4 = st.columns(4)
  with col1:
    st.metric(label="Active Crops", value="12 Varieties", delta="+2 this month")
  with col2:
    st.metric(label="Livestock Count", value="45 Animals", delta="0")
  with col3:
    st.metric(label="Soil Moisture", value="68%", delta="-4% (Needs water)")
  with col4:
    st.metric(label="Pending Alerts", value="2 Warnings", delta="-1")

  st.markdown("---")
  st.subheader("🌦️ Local Weather & Farm Advisory")
  st.info(
      "**Tip for today:** Excellent weather for scouting pests on Solanaceae"
      " crops. Ensure drainage ditches around the onion and garlic plots are"
      " clear."
  )

# -------------------------------------------------------------
# TAB 2: CROP & GARDEN MANAGER
# -------------------------------------------------------------
elif app_mode == "Crop & Garden Manager":
  st.subheader("🌿 Crop Growth & Plot Tracker")

  crop_name = st.selectbox(
      "Select Crop Plot",
      [
          "Sukuma Wiki / Spinach",
          "Onions & Garlic",
          "Beetroot & Beans",
          "Passion Fruit & Pawpaw",
      ],
  )

  col1, col2 = st.columns(2)
  with col1:
    planting_date = st.date_input("Planting Date")
    stage = st.selectbox(
        "Growth Stage", ["Seedling", "Vegetative", "Flowering", "Harvesting"]
    )
  with col2:
    fertilizer_applied = st.text_input(
        "Last Fertilizer / Compost Application"
    )
    notes = st.text_area("Field Observations")

  if st.button("Save Crop Record"):
    st.success(f"Successfully updated records for {crop_name}!")

# -------------------------------------------------------------
# TAB 3: AI PEST & DISEASE DIAGNOSTIC
# -------------------------------------------------------------
elif app_mode == "AI Pest & Disease Diagnostic":
  st.subheader("🔍 AI Crop Doctor & Bio-Pesticide Advisor")
  st.markdown(
      "Upload a photo or video of the affected leaf, stem, or fruit to get an"
      " instant diagnosis and recommended organic remedy formula."
  )

  uploaded_file = st.file_uploader(
      "Upload Plant Image", type=["jpg", "jpeg", "png"]
  )

  if uploaded_file is not None:
    image = Image.open(uploaded_file)
    st.image(
        image, caption="Uploaded Crop Specimen", use_container_width=True
    )

    if st.button("Run AI Diagnosis"):
      with st.spinner("Analyzing plant health and pattern matching..."):
        # Simulated AI Diagnostic Output (Ready to plug in Google GenAI SDK)
        st.warning("⚠️ **Detected Issue:** Early Blight (*Alternaria solani*)")
        st.markdown("""
                ### Recommended Bio-Pesticide Remedy Formula:
                1. **Garlic & Neem Extract Spray:** Crush 50g of garlic cloves and 100g of fresh neem leaves, steep in 1 litre of water overnight, strain, and dilute with 4 litres of water.
                2. **Application:** Spray thoroughly on both sides of the affected leaves early in the morning or late evening.
                3. **Frequency:** Repeat every 5 to 7 days until symptoms subside.
                """)

# -------------------------------------------------------------
# TAB 4: LIVESTOCK & FEED TRACKER
# -------------------------------------------------------------
elif app_mode == "Livestock & Feed Tracker":
  st.subheader("🐄 Livestock Health & Feed Management")
  tab1, tab2 = st.tabs(["Livestock Records", "Feed Inventory"])

  with tab1:
    st.write("Manage animal vaccination schedules, weights, and health notes.")
    animal_id = st.text_input("Animal ID / Tag Number")
    health_status = st.selectbox(
        "Status", ["Healthy", "Under Observation", "Quarantined"]
    )
    if st.button("Log Health Update"):
      st.success(f"Health log saved for {animal_id}")

  with tab2:
    st.write("Track stock levels for protein feeds, mineral salts, and hay.")
    st.progress(0.75, text="Feed Store Capacity: 75% Full")

# -------------------------------------------------------------
# TAB 5: OFFLINE SYNC STATUS
# -------------------------------------------------------------
elif app_mode == "Offline Sync Status":
  st.subheader("📡 Offline Mode & Synchronization")
  st.markdown(
      "FarmAssist caches your offline inputs locally when network signal is"
      " low."
  )

  if st.button("Check Connection & Sync Data"):
    st.success(
        "All local records are synchronized successfully with the cloud"
        " database!"
    )
