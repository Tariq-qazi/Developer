import streamlit as st
import pandas as pd

# Load developer summary with matched developers
df = pd.read_csv("data/developer_summary_with_developers.csv")

# Streamlit App Title
st.title("📊 Property Growth Intelligence Advisor")

# Section Selection
section = st.sidebar.radio("Choose a section:", [
    "🏘️ Evaluate My Property",
    "📈 Compare Better Growth Options",
    "💬 GPT: Ask an Investment Question (Coming Soon)"
])

# Shared growth rate map (estimate only, you can replace with historical if available)
stage_growth_map = {
    "Off-Plan or Launch": 0.06,
    "Early Growth": 0.04,
    "Maturity": 0.025,
    "Stabilized / Legacy": 0.012
}

# Section 1: Evaluate My Property
if section == "🏘️ Evaluate My Property":
    st.header("🏘️ Property Evaluation")
    st.write("Select details of a unit to analyze its developer's growth potential.")

    developer = st.selectbox("Developer Name", sorted(df['developer_name'].dropna().unique()))
    dev_df = df[df['developer_name'] == developer]

    master = st.selectbox("Master Project", sorted(dev_df['master_project_en'].dropna().unique()))
    prop_type = st.selectbox("Property Type", sorted(dev_df['property_type_en'].dropna().unique()))

    match = dev_df[(dev_df['master_project_en'] == master) & (dev_df['property_type_en'] == prop_type)]

    if not match.empty:
        st.markdown(f"### Insights for **{master}** by **{developer}**")
        for _, row in match.iterrows():
            st.markdown(f"**Growth Stage**: {row['growth_stage']}  ")
            st.markdown(f"Median Price: AED {int(row['median_price']):,}  ")
            st.markdown(f"Price per sqm: AED {int(row['price_per_sqm']):,}  ")
            st.markdown(f"Transaction Count: {row['transaction_count']}  ")

            rate = stage_growth_map.get(row['growth_stage'], 0.02)
            st.success(f"📈 Est. 5-Year Appreciation: {round(rate * 5 * 100, 1)}%  | 10-Year: {round(rate * 10 * 100, 1)}%")
            st.markdown("---")

# Section 2: Compare Better Growth Options
elif section == "📈 Compare Better Growth Options":
    st.header("📈 Compare Developers or Master Projects")

    prop_type = st.selectbox("Select Property Type", sorted(df['property_type_en'].dropna().unique()))
    stage_filter = st.multiselect("Filter by Growth Stage", sorted(df['growth_stage'].dropna().unique()))

    filtered = df[df['property_type_en'] == prop_type]
    if stage_filter:
        filtered = filtered[filtered['growth_stage'].isin(stage_filter)]

    top_projects = filtered.sort_values(by='price_per_sqm', ascending=False).head(10)

    st.markdown("### 🔝 Top 10 Projects by Price per sqm")
    for _, row in top_projects.iterrows():
        rate = stage_growth_map.get(row['growth_stage'], 0.02)
        st.markdown(f"""
        **{row['master_project_en']}** by *{row['developer_name']}*  
        Growth Stage: {row['growth_stage']}  
        Price/sqm: AED {int(row['price_per_sqm'])}  
        Est. 5Y Appreciation: {round(rate * 5 * 100, 1)}%  
        Transactions: {row['transaction_count']}  
        ---
        """)

# Section 3: GPT (Coming Soon)
elif section == "💬 GPT: Ask an Investment Question (Coming Soon)":
    st.header("💬 Smart Investment Assistant")
    st.info("This section will allow natural language queries like 'Compare Emaar and Damac for 2BRs in Business Bay'. Coming soon!")
