import streamlit as st
import numpy as np
import matplotlib.pyplot as plt
import librosa
import librosa.display
import hashlib
import time

# --- 1. BRANDING & SETUP ---
st.set_page_config(page_title="RESONANCE", layout="centered")

# CSS: DARK LUXURY THEME
st.markdown("""
    <style>
    /* Main Background */
    .stApp {
        background-color: #050505;
        color: #ffffff;
        font-family: 'Helvetica Neue', sans-serif;
    }
    /* Headers */
    h1, h2, h3 {
        font-weight: 300 !important;
        letter-spacing: 0.1em !important;
        text-transform: uppercase;
    }
    /* Metrics */
    div[data-testid="stMetricValue"] {
        font-size: 40px;
        color: #00f2ff; /* ELECTRIC BLUE ACCENT */
    }
    /* Buttons */
    .stButton > button {
        background-color: #1a1a1a;
        color: white;
        border: 1px solid #333;
        border-radius: 0px;
        width: 100%;
    }
    .stButton > button:hover {
        border-color: #00f2ff;
        color: #00f2ff;
    }
    </style>
    """, unsafe_allow_html=True)

# --- 2. HEADER (THE LOGO) ---
st.markdown("<br><h1 style='text-align: center;'>R E S O N A N C E</h1>", unsafe_allow_html=True)
st.markdown("<p style='text-align: center; color: #666; font-size: 12px;'>AUDIO INTELLIGENCE // V.1.0</p><br>", unsafe_allow_html=True)

# --- 3. THE ENGINE ---
tab1, tab2 = st.tabs(["MARKET SIMULATION", "LAB ANALYSIS (UPLOAD)"])

# TAB 1: SIMULATION (For Pitching)
with tab1:
    st.write("Generate strategic reports for known tracks.")
    col1, col2 = st.columns([2, 1])
    with col1:
        track_input = st.text_input("TRACK NAME", placeholder="e.g. POWER")
    with col2:
        artist_input = st.text_input("ARTIST", placeholder="e.g. KANYE WEST")
    
    run_btn = st.button("ANALYZE SIGNAL")

    if run_btn and track_input:
        with st.spinner("DECODING CULTURAL SIGNATURE..."):
            time.sleep(1.5) # Fake processing delay for effect
            
            # Deterministic Math (Same Input = Same Score)
            seed = track_input.upper() + artist_input.upper()
            hash_val = int(hashlib.sha256(seed.encode('utf-8')).hexdigest(), 16)
            score = (hash_val % 70) + 30
            bpm = (hash_val % 50) + 90
            
            # Strategy Logic
            if score > 80:
                strat = "HERITAGE / LUXURY"
                desc = "High alignment with established luxury codes. Use for Brand Campaigns."
            elif score > 50:
                strat = "CONTEMPORARY / RETAIL"
                desc = "High kinetic energy. Optimized for Social Media & In-Store."
            else:
                strat = "AVANT-GARDE / NICHE"
                desc = "Disruptive textures. Use for Runway or Teasers."

            st.markdown("---")
            c1, c2, c3 = st.columns(3)
            c1.metric("RESONANCE", f"{score}/100")
            c2.metric("BPM", bpm)
            c3.metric("MOOD", "HIGH" if score > 50 else "DARK")
            
            st.success(f"STRATEGY: {strat}")
            st.caption(desc)
            
            # Generative Visual
            fig, ax = plt.subplots(figsize=(10, 2))
            x = np.linspace(0, 20, 200)
            y = np.sin(x) * np.random.rand(200)
            ax.plot(x, y, color='#00f2ff', linewidth=1)
            ax.set_axis_off()
            fig.patch.set_facecolor('#050505')
            ax.set_facecolor('#050505')
            st.pyplot(fig)

# TAB 2: REAL UPLOAD
with tab2:
    uploaded_file = st.file_uploader("UPLOAD AUDIO (MP3/WAV)", type=['mp3', 'wav'])
    if uploaded_file is not None:
        with st.spinner("INGESTING RAW DATA..."):
            # Save temp
            with open("temp.mp3", "wb") as f:
                f.write(uploaded_file.getbuffer())
            
            y, sr = librosa.load("temp.mp3", duration=30)
            tempo, _ = librosa.beat.beat_track(y=y, sr=sr)
            
            st.markdown("---")
            st.metric("DETECTED BPM", int(tempo))
            
            fig, ax = plt.subplots(figsize=(10, 2))
            librosa.display.waveshow(y, sr=sr, color='#00f2ff', alpha=0.8)
            ax.set_axis_off()
            fig.patch.set_facecolor('#050505')
            ax.set_facecolor('#050505')
            st.pyplot(fig)