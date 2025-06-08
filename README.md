# smartCV-resume-bot
# app.py
import streamlit as st
from fpdf import FPDF
import datetime

st.set_page_config(page_title="SmartCV Bot", layout="centered")

st.title("📄 SmartCV Bot - AI Resume + Cover Letter Generator")

# === Form ===
with st.form("resume_form"):
    st.subheader("📝 Enter Your Details")
    name = st.text_input("Full Name")
    phone = st.text_input("Phone Number")
    email = st.text_input("Email Address")
    job_title = st.text_input("Target Job Title")
    summary = st.text_area("Career Summary", height=100)
    education = st.text_area("Education Background", height=100)
    experience = st.text_area("Work Experience", height=100)
    skills = st.text_area("Key Skills", height=80)
    submit = st.form_submit_button("Generate Resume & Cover Letter")

# === PDF Generation ===
def generate_resume(name, phone, email, job_title, summary, education, experience, skills):
    pdf = FPDF()
    pdf.add_page()
    pdf.set_font("Arial", size=12)

    pdf.set_font("Arial", "B", size=14)
    pdf.cell(200, 10, txt=name, ln=True, align='C')
    pdf.set_font("Arial", size=12)
    pdf.cell(200, 10, txt=f"{phone} | {email}", ln=True, align='C')

    pdf.set_font("Arial", "B", 12)
    pdf.cell(200, 10, txt="Career Objective", ln=True)
    pdf.set_font("Arial", size=12)
    pdf.multi_cell(0, 10, summary)

    pdf.set_font("Arial", "B", 12)
    pdf.cell(200, 10, txt="Education", ln=True)
    pdf.set_font("Arial", size=12)
    pdf.multi_cell(0, 10, education)

    pdf.set_font("Arial", "B", 12)
    pdf.cell(200, 10, txt="Experience", ln=True)
    pdf.set_font("Arial", size=12)
    pdf.multi_cell(0, 10, experience)

    pdf.set_font("Arial", "B", 12)
    pdf.cell(200, 10, txt="Skills", ln=True)
    pdf.set_font("Arial", size=12)
    pdf.multi_cell(0, 10, skills)

    return pdf

def generate_cover_letter(name, job_title, summary):
    letter = FPDF()
    letter.add_page()
    letter.set_font("Arial", size=12)
    letter.multi_cell(0, 10, f"Dear Hiring Manager,\n\nI am excited to apply for the position of {job_title}.")
    letter.multi_cell(0, 10, summary)
    letter.multi_cell(0, 10, f"I believe my skills and background make me a strong fit for this role.\n\nSincerely,\n{name}")
    return letter

if submit:
    resume = generate_resume(name, phone, email, job_title, summary, education, experience, skills)
    resume_file = f"{name.replace(' ', '_')}_Resume.pdf"
    resume.output(resume_file)

    letter = generate_cover_letter(name, job_title, summary)
    letter_file = f"{name.replace(' ', '_')}_CoverLetter.pdf"
    letter.output(letter_file)

    st.success("✅ Documents generated successfully!")
    with open(resume_file, "rb") as f:
        st.download_button("📄 Download Resume", f, file_name=resume_file)

    with open(letter_file, "rb") as f:
        st.download_button("✉ Download Cover Letter", f, file_name=letter_file)
