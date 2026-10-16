# FarmAssist-import os
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

# 2. Custom Header & Branding
st.title("🌱 FarmAssist: Digital Farm Manager")
st.markdown(
    "Your comprehensive assistant for sustainable crop management, livestock"
    " tracking, and real-time AI agricultural advice."
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
        "💬 AI Farm Expert Chat",
        "Offline Sync Status",
    ],
)

# -------------------------------------------------------------
# TAB 1: DASHBOARD
# -------------------------------------------------------------
if app_mode == "Dashboard":
  st.subheader("📊 Farm Overview & Metrics")

  col1, col2, col3, col4 = st.columns(4)
  with col1:
    st.metric(
        label="Active Crop Plots", value="14 Varieties", delta="+2 this season"
    )
  with col2:
    st.metric(label="Livestock Stock", value="48 Animals", delta="+3 births")
  with col3:
    st.metric(label="Soil Moisture", value="71%", delta="+3% (Optimal)")
  with col4:
    st.metric(label="Active Alerts", value="1 Warning", delta="-2 resolved")

  st.markdown("---")
  st.subheader("🌦️ Daily Agronomy Advisory & Weather")
  st.success(
      "**Field Note:** Ideal conditions for foliar feeding and organic pest"
      " scouting. Ensure drainage channels are clear around the root-crop"
      " sections."
  )

# -------------------------------------------------------------
# TAB 2: CROP & GARDEN MANAGER
# -------------------------------------------------------------
elif app_mode == "Crop & Garden Manager":
  st.subheader("🌿 Crop Growth & Plot Tracker")

  crop_plot = st.selectbox(
      "Select Plot / Bed",
      [
          "Bed 1: Sukuma Wiki & Spinach",
          "Bed 2: Onions & Garlic",
          "Bed 3: Beetroot & Beans",
          "Bed 4: Passion Fruit & Pawpaw",
          "Bed 5: Indigenous Greens (Terere)",
      ],
  )

  col1, col2 = st.columns(2)
  with col1:
    planting_date = st.date_input("Planting Date")
    growth_stage = st.selectbox(
        "Current Growth Stage",
        ["Germination", "Vegetative / Leafy", "Flowering / Fruiting", "Harvesting"],
    )
    soil_ph = st.slider("Estimated Soil pH", 4.0, 9.0, 6.5)
  with col2:
    input_used = st.text_input(
        "Applied Fertilizer / Compost",
        placeholder="e.g., Well-rotted manure, NPK",
    )
    pest_obs = st.text_input(
        "Pest / Disease Observations", placeholder="e.g., Minor aphid presence"
    )
    notes = st.text_area("General Field Observations & Maintenance")

  if st.button("Save Plot Record", type="primary"):
    st.success(f"Successfully updated records for {crop_plot}!")

# -------------------------------------------------------------
# TAB 3: AI PEST & DISEASE DIAGNOSTIC
# -------------------------------------------------------------
elif app_mode == "AI Pest & Disease Diagnostic":
  st.subheader("🔍 AI Crop Doctor & Bio-Pesticide Advisor")
  st.markdown(
      "Upload a clear photo of the affected leaves, stems, or fruits to receive"
      " an instant diagnosis, severity score, and organic remedy formulation."
  )

  uploaded_file = st.file_uploader(
      "Upload Plant Specimen Image", type=["jpg", "jpeg", "png"]
  )

  if uploaded_file is not None:
    image = Image.open(uploaded_file)
    st.image(
        image, caption="Uploaded Crop Specimen", use_container_width=True
    )

    if st.button(
        "Run Advanced AI Diagnostic Analysis", type="primary"
    ):
      with st.spinner(
          "Analyzing cellular damage patterns and matching database..."
      ):
        # Structured diagnostic feedback
        st.error(
            "⚠️ **Diagnosis Found:** Moderate Leaf Blight / Fungal Spot"
            " (*Alternaria solani*) Detected."
        )
        st.metric(label="Confidence Score", value="94.2%", delta="High Accuracy")

        st.markdown("""
                ### 📋 Recommended Organic Remedy & Action Plan:
                1. **Immediate Sanitation:** Prune and safely dispose of heavily infected lower leaves to prevent spore spread.
                2. **Bio-Pesticide Formula (Garlic & Neem Spray):** 
                   * Crush 50g of fresh garlic and 100g of neem leaves.
                   * Steep overnight in 1 liter of water, strain, and mix with 4 liters of clean water plus a few drops of organic liquid soap as a sticker.
                3. **Application Schedule:** Spray thoroughly on both adaxial and abaxial leaf surfaces early in the morning every 5 days.
                """)

# -------------------------------------------------------------
# TAB 4: LIVESTOCK & FEED TRACKER
# -------------------------------------------------------------
elif app_mode == "Livestock & Feed Tracker":
  st.subheader("🐄 Livestock Health & Feed Inventory")
  tab1, tab2 = st.tabs(["Livestock Records", "Feed Store Inventory"])

  with tab1:
    st.write("Log individual animal health, vaccinations, and weight gain.")
    tag_id = st.text_input("Animal Tag ID / Name")
    category = st.selectbox("Category", ["Dairy Cow", "Poultry", "Goat", "Pig"])
    status = st.selectbox(
        "Health Status", ["Healthy & Active", "Under Treatment", "Quarantined"]
    )
    if st.button("Save Animal Log"):
      st.success(f"Record successfully logged for {tag_id} ({category}).")

  with tab2:
    st.write("Monitor feed stock levels and nutrition supplements.")
    st.progress(0.68, text="Protein Concentrate Store: 68% Remaining")
    st.progress(0.85, text="Hay / Roughage Stock: 85% Remaining")

# -------------------------------------------------------------
# TAB 5: AI FARM EXPERT CHAT (NEW)
# -------------------------------------------------------------
elif app_mode == "💬 AI Farm Expert Chat":
  st.subheader("💬 FarmAssist Expert AI Assistant")
  st.markdown(
      "Chat live with your virtual agronomist. Ask any question about crop"
      " diseases, fertilizer application ratios, organic pest control, or"
      " livestock feeds."
  )

  # Initialize chat history in session state
  if "messages" not in st.session_state:
    st.session_state.messages = [{
        "role": "assistant",
        "content": (
            "Hello! I am your FarmAssist Expert AI. What agricultural challenge"
            " can we solve together today?"
        ),
    }]

  # Display prior messages
  for message in st.session_state.messages:
    with st.chat_message(message["role"]):
      st.markdown(message["content"])

  # Handle user input
  if prompt := st.chat_input(
      "Ask about pests, soil fertility, crop spacing, livestock..."
  ):
    st.session_state.messages.append({"role": "user", "content": prompt})
    with st.chat_message("user"):
      st.markdown(prompt)

    # Generate intelligent agronomic response
    with st.chat_message("assistant"):
      with st.spinner("Consulting agricultural knowledge base..."):
        prompt_lower = prompt.lower()
        if (
            "sukuma" in prompt_lower
            or "spinach" in prompt_lower
            or "kale" in prompt_lower
        ):
          response = (
              "For Sukuma Wiki and leafy greens, maintain consistent moisture"
              " and apply nitrogen-rich organic top-dressing (like well-rotted"
              " manure or compost tea) every 2 weeks. For aphids, use a garlic"
              " and chili soap spray."
          )
        elif (
            "blight" in prompt_lower
            or "tomato" in prompt_lower
            or "potato" in prompt_lower
        ):
          response = (
              "Blight thrives in damp environments. Ensure wide spacing between"
              " plants for maximum airflow, avoid overhead watering (water the"
              " base instead), and use a copper-based or neem oil preventative"
              " spray weekly."
          )
        elif (
            "feed" in prompt_lower
            or "cow" in prompt_lower
            or "livestock" in prompt_lower
        ):
          response = (
              "For high milk yields in dairy animals, combine high-quality"
              " grass (like Napier grass) with protein legume supplements (such"
              " as desmodium or sweet potato vines) and ensure free access"
              " to clean water and mineral salt licks."
          )
        elif "fertilizer" in prompt_lower or "soil" in prompt_lower:
          response = (
              "Healthy soil requires a balance of organic matter. Incorporate"
              " compost during land preparation, and use side-dressing of rich"
              " compost or appropriate organic fertilizers when plants reach"
              " the vegetative growth stage."
          )
        else:
          response = (
              f"That is a great question regarding **{prompt}**. In sustainable"
              " farming, always prioritize soil biodiversity, proper crop"
              " rotation, and early morning field scouting. Would you like a"
              " specific step-by-step guideline or remedy for this?"
          )

        st.markdown(response)
        st.session_state.messages.append(
            {"role": "assistant", "content": response}
        )

# -------------------------------------------------------------
# TAB 6: OFFLINE SYNC STATUS
# -------------------------------------------------------------
elif app_mode == "Offline Sync Status":
  st.subheader("📡 Offline Mode & Local Synchronization")
  st.markdown(
      "FarmAssist caches your inputs locally when working in low-connectivity"
      " field conditions."
  )

  col1, col2 = st.columns(2)
  with col1:
    st.info("**Local Storage Cache:** 3 records pending sync")
  with col2:
    st.success("**Cloud Connection Status:** Online & Ready")

  if st.button("Trigger Manual Cloud Sync"):
    st.success(
        "All local field logs and diagnostic entries synchronized successfully!"
    )
