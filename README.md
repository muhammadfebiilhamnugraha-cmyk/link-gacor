import streamlit as st

st.set_page_config(
    page_title="Kalkulator Kimia Analisis",
    page_icon="⚗️",
    layout="centered"
)

# =========================
# CSS BACKGROUND & STYLE
# =========================
st.markdown(
    """
    <style>
    /* Background utama */
    .stApp {
        background: linear-gradient(
            to right,
            #e3f2fd,
            #ffffff
        );
    }

    /* Card style */
    .card {
        background-color: white;
        padding: 25px;
        border-radius: 12px;
        box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.1);
        margin-bottom: 20px;
    }

    /* Judul */
    h1, h2, h3 {
        color: #0d47a1;
    }
    </style>
    """,
    unsafe_allow_html=True
)

st.title("🧪 Selamat datang di Kalkulator Kimia Analisis")
st.write("### Silahkan pilih menu di bawah ini")

menu = st.selectbox(
    "Pilih jenis perhitungan:",
    (
        "Faktor Pengenceran",
        "Normalitas",
        "Molaritas",
        "Mg/L",
        "% b/v",
        "% b/b",
        "% v/v"
    )
)

st.markdown("---")

# =========================
# MENU FAKTOR PENGENCERAN
# =========================
if menu == "Faktor Pengenceran":

    st.subheader("⚗️ Faktor Pengenceran")

    sub_menu = st.radio(
        "Pilih perhitungan:",
        (
            "Faktor pengenceran",
            "Volume yang harus diambil"
        )
    )

    st.markdown("---")

    # 1. Faktor Pengenceran
    if sub_menu == "Faktor pengenceran":
        st.write("### Faktor pengenceran")

        volume_labu = st.number_input(
            "Masukkan volume labu takar yang ingin digunakan (mL)",
            min_value=0.0,
            format="%.3f"
        )

        volume_pipet = st.number_input(
            "Masukkan volume yang dipipet (mL)",
            min_value=0.0,
            format="%.3f"
        )

        if st.button("Hitung Faktor Pengenceran"):
            if volume_pipet == 0:
                st.error("Volume yang dipipet tidak boleh 0.")
            else:
                faktor_pengenceran = volume_labu / volume_pipet
                st.success(f"✅ Faktor pengenceran = **{faktor_pengenceran:.3f}**")

    # 2. Volume yang Harus Diambil
    elif sub_menu == "Volume yang harus diambil":
        st.write("### Volume yang harus diambil")

        konsentrasi_awal = st.number_input(
            "Masukkan konsentrasi larutan yang ingin diambil",
            min_value=0.0,
            format="%.3f"
        )

        konsentrasi_akhir = st.number_input(
            "Masukkan konsentrasi larutan yang ingin dibuat",
            min_value=0.0,
            format="%.3f"
        )

        volume_akhir = st.number_input(
            "Masukkan volume larutan yang ingin dibuat (mL)",
            min_value=0.0,
            format="%.3f"
        )

        if st.button("Hitung Volume yang Harus Diambil"):
            if konsentrasi_awal == 0:
                st.error("Konsentrasi larutan yang ingin diambil tidak boleh 0.")
            else:
                volume_diambil = (volume_akhir * konsentrasi_akhir) / konsentrasi_awal
                st.success(
                    f"✅ Volume larutan yang harus diambil = **{volume_diambil:.3f} mL**"
                )

# =========================
# DATABASE Ar / Mr SENYAWA
# =========================
mr_database = {
    "NaCl": 58.44,
    "HCl": 36.46,
    "H2SO4": 98.08,
    "NaOH": 40.00,
    "KOH": 56.11,
    "CH3COOH": 60.05,
    "NH3": 17.03,
    "KMnO4": 158.04,
    "AgNO3": 169.87,
    "CaCO3": 100.09
}

# =========================
# MENU MOLARITAS
# =========================
if menu == "Molaritas":

    st.subheader("⚗️ Perhitungan Molaritas")

    metode = st.radio(
        "Pilih metode perhitungan:",
        (
            "Menggunakan massa zat",
            "Menggunakan Mr dari database"
        )
    )

    st.markdown("---")

    volume_larutan = st.number_input(
        "Masukkan volume larutan (L)",
        min_value=0.0,
        format="%.3f"
    )

    if metode == "Menggunakan massa zat":
        massa = st.number_input(
            "Masukkan massa zat (gram)",
            min_value=0.0,
            format="%.4f"
        )

        mr = st.number_input(
            "Masukkan Mr zat",
            min_value=0.0,
            format="%.3f"
        )

        if st.button("Hitung Molaritas"):
            if volume_larutan == 0 or mr == 0:
                st.error("Volume dan Mr tidak boleh 0.")
            else:
                molaritas = massa / (mr * volume_larutan)
                st.success(f"✅ Molaritas = **{molaritas:.4f} M**")

    elif metode == "Menggunakan Mr dari database":
        senyawa = st.selectbox(
            "Pilih senyawa:",
            list(mr_database.keys())
        )

        massa = st.number_input(
            "Masukkan massa zat (gram)",
            min_value=0.0,
            format="%.4f"
        )

        mr = mr_database[senyawa]
        st.info(f"Mr {senyawa} = {mr}")

        if st.button("Hitung Molaritas"):
            if volume_larutan == 0:
                st.error("Volume larutan tidak boleh 0.")
            else:
                molaritas = massa / (mr * volume_larutan)
                st.success(f"✅ Molaritas = **{molaritas:.4f} M**")

# =========================
# MENU NORMALITAS
# =========================
if menu == "Normalitas":

    st.subheader("⚗️ Perhitungan Normalitas")

    metode = st.radio(
        "Pilih metode perhitungan:",
        (
            "Menggunakan massa zat",
            "Menggunakan Mr dari database"
        )
    )

    st.markdown("---")

    volume_larutan = st.number_input(
        "Masukkan volume larutan (L)",
        min_value=0.0,
        format="%.3f"
    )

    faktor_ekivalen = st.number_input(
        "Masukkan faktor ekivalen (n)",
        min_value=0.0,
        format="%.2f",
        help="Contoh: HCl = 1, H2SO4 = 2, NaOH = 1"
    )

    if metode == "Menggunakan massa zat":
        massa = st.number_input(
            "Masukkan massa zat (gram)",
            min_value=0.0,
            format="%.4f"
        )

        mr = st.number_input(
            "Masukkan Mr zat",
            min_value=0.0,
            format="%.3f"
        )

        if st.button("Hitung Normalitas"):
            if volume_larutan == 0 or mr == 0 or faktor_ekivalen == 0:
                st.error("Volume, Mr, dan faktor ekivalen tidak boleh 0.")
            else:
                normalitas = (massa * faktor_ekivalen) / (mr * volume_larutan)
                st.success(f"✅ Normalitas = **{normalitas:.4f} N**")

    elif metode == "Menggunakan Mr dari database":
        senyawa = st.selectbox(
            "Pilih senyawa:",
            list(mr_database.keys())
        )

        massa = st.number_input(
            "Masukkan massa zat (gram)",
            min_value=0.0,
            format="%.4f"
        )

        mr = mr_database[senyawa]
        st.info(f"Mr {senyawa} = {mr}")

        if st.button("Hitung Normalitas"):
            if volume_larutan == 0 or faktor_ekivalen == 0:
                st.error("Volume dan faktor ekivalen tidak boleh 0.")
            else:
                normalitas = (massa * faktor_ekivalen) / (mr * volume_larutan)
                st.success(f"✅ Normalitas = **{normalitas:.4f} N**")

# =========================
# MENU mg/L
# =========================
if menu == "Mg/L":

    st.subheader("⚗️ Perhitungan Konsentrasi mg/L")

    sub_menu = st.radio(
        "Pilih jenis perhitungan:",
        (
            "Cari konsentrasi (mg/L)",
            "Cari massa (mg)",
            "Cari volume (L)"
        )
    )

    st.markdown("---")

    # 1. Cari konsentrasi mg/L
    if sub_menu == "Cari konsentrasi (mg/L)":
        st.write("### Menghitung konsentrasi (mg/L)")

        massa = st.number_input(
            "Masukkan massa zat (mg)",
            min_value=0.0,
            format="%.4f"
        )

        volume = st.number_input(
            "Masukkan volume larutan (L)",
            min_value=0.0,
            format="%.4f"
        )

        if st.button("Hitung Konsentrasi"):
            if volume == 0:
                st.error("Volume tidak boleh 0.")
            else:
                konsentrasi = massa / volume
                st.success(f"✅ Konsentrasi = **{konsentrasi:.4f} mg/L**")

    # 2. Cari massa
    elif sub_menu == "Cari massa (mg)":
        st.write("### Menghitung massa zat (mg)")

        konsentrasi = st.number_input(
            "Masukkan konsentrasi (mg/L)",
            min_value=0.0,
            format="%.4f"
        )

        volume = st.number_input(
            "Masukkan volume larutan (L)",
            min_value=0.0,
            format="%.4f"
        )

        if st.button("Hitung Massa"):
            massa = konsentrasi * volume
            st.success(f"✅ Massa zat = **{massa:.4f} mg**")

    # 3. Cari volume
    elif sub_menu == "Cari volume (L)":
        st.write("### Menghitung volume larutan (L)")

        konsentrasi = st.number_input(
            "Masukkan konsentrasi (mg/L)",
            min_value=0.0,
            format="%.4f"
        )

        massa = st.number_input(
            "Masukkan massa zat (mg)",
            min_value=0.0,
            format="%.4f"
        )

        if st.button("Hitung Volume"):
            if konsentrasi == 0:
                st.error("Konsentrasi tidak boleh 0.")
            else:
                volume = massa / konsentrasi
                st.success(f"✅ Volume larutan = **{volume:.4f} L**")

if menu == "% b/v":

    st.subheader("⚗️ Persen berat/volume (% b/v)")
    st.write("% b/v = gram zat terlarut per 100 mL larutan")

    sub_menu = st.radio(
        "Pilih perhitungan:",
        (
            "Cari % b/v",
            "Cari massa zat (g)",
            "Cari volume larutan (mL)"
        )
    )

    st.markdown("---")

    # 1. Cari % b/v
    if sub_menu == "Cari % b/v":
        massa = st.number_input(
            "Masukkan massa zat (gram)",
            min_value=0.0,
            format="%.4f"
        )
        volume = st.number_input(
            "Masukkan volume larutan (mL)",
            min_value=0.0,
            format="%.2f"
        )

        if st.button("Hitung % b/v"):
            if volume == 0:
                st.error("Volume tidak boleh 0.")
            else:
                persen = (massa / volume) * 100
                st.success(f"✅ Konsentrasi = **{persen:.4f} % b/v**")

    # 2. Cari massa
    elif sub_menu == "Cari massa zat (g)":
        persen = st.number_input(
            "Masukkan konsentrasi (% b/v)",
            min_value=0.0,
            format="%.4f"
        )
        volume = st.number_input(
            "Masukkan volume larutan (mL)",
            min_value=0.0,
            format="%.2f"
        )

        if st.button("Hitung Massa"):
            massa = (persen / 100) * volume
            st.success(f"✅ Massa zat = **{massa:.4f} g**")

    # 3. Cari volume
    elif sub_menu == "Cari volume larutan (mL)":
        persen = st.number_input(
            "Masukkan konsentrasi (% b/v)",
            min_value=0.0,
            format="%.4f"
        )
        massa = st.number_input(
            "Masukkan massa zat (gram)",
            min_value=0.0,
            format="%.4f"
        )

        if st.button("Hitung Volume"):
            if persen == 0:
                st.error("Konsentrasi tidak boleh 0.")
            else:
                volume = (massa * 100) / persen
                st.success(f"✅ Volume larutan = **{volume:.2f} mL**")

if menu == "% b/b":

    st.subheader("⚗️ Persen berat/berat (% b/b)")
    st.write("% b/b = gram zat terlarut per 100 gram campuran")

    sub_menu = st.radio(
        "Pilih perhitungan:",
        (
            "Cari % b/b",
            "Cari massa zat (g)",
            "Cari massa campuran (g)"
        )
    )

    st.markdown("---")

    # 1. Cari % b/b
    if sub_menu == "Cari % b/b":
        massa_zat = st.number_input(
            "Masukkan massa zat terlarut (gram)",
            min_value=0.0,
            format="%.4f"
        )
        massa_total = st.number_input(
            "Masukkan massa campuran (gram)",
            min_value=0.0,
            format="%.4f"
        )

        if st.button("Hitung % b/b"):
            if massa_total == 0:
                st.error("Massa campuran tidak boleh 0.")
            else:
                persen = (massa_zat / massa_total) * 100
                st.success(f"✅ Konsentrasi = **{persen:.4f} % b/b**")

    # 2. Cari massa zat
    elif sub_menu == "Cari massa zat (g)":
        persen = st.number_input(
            "Masukkan konsentrasi (% b/b)",
            min_value=0.0,
            format="%.4f"
        )
        massa_total = st.number_input(
            "Masukkan massa campuran (gram)",
            min_value=0.0,
            format="%.4f"
        )

        if st.button("Hitung Massa Zat"):
            massa_zat = (persen / 100) * massa_total
            st.success(f"✅ Massa zat terlarut = **{massa_zat:.4f} g**")

    # 3. Cari massa campuran
    elif sub_menu == "Cari massa campuran (g)":
        persen = st.number_input(
            "Masukkan konsentrasi (% b/b)",
            min_value=0.0,
            format="%.4f"
        )
        massa_zat = st.number_input(
            "Masukkan massa zat terlarut (gram)",
            min_value=0.0,
            format="%.4f"
        )

        if st.button("Hitung Massa Campuran"):
            if persen == 0:
                st.error("Konsentrasi tidak boleh 0.")
            else:
                massa_total = (massa_zat * 100) / persen
                st.success(f"✅ Massa campuran = **{massa_total:.4f} g**")

if menu == "% v/v":

    st.subheader("⚗️ Persen volume/volume (% v/v)")
    st.write("% v/v = mL zat terlarut per 100 mL larutan")

    sub_menu = st.radio(
        "Pilih perhitungan:",
        (
            "Cari % v/v",
            "Cari volume zat (mL)",
            "Cari volume larutan (mL)"
        )
    )

    st.markdown("---")

    # 1. Cari % v/v
    if sub_menu == "Cari % v/v":
        volume_zat = st.number_input(
            "Masukkan volume zat terlarut (mL)",
            min_value=0.0,
            format="%.2f"
        )
        volume_total = st.number_input(
            "Masukkan volume larutan (mL)",
            min_value=0.0,
            format="%.2f"
        )

        if st.button("Hitung % v/v"):
            if volume_total == 0:
                st.error("Volume larutan tidak boleh 0.")
            else:
                persen = (volume_zat / volume_total) * 100
                st.success(f"✅ Konsentrasi = **{persen:.4f} % v/v**")

    # 2. Cari volume zat
    elif sub_menu == "Cari volume zat (mL)":
        persen = st.number_input(
            "Masukkan konsentrasi (% v/v)",
            min_value=0.0,
            format="%.4f"
        )
        volume_total = st.number_input(
            "Masukkan volume larutan (mL)",
            min_value=0.0,
            format="%.2f"
        )

        if st.button("Hitung Volume Zat"):
            volume_zat = (persen / 100) * volume_total
            st.success(f"✅ Volume zat terlarut = **{volume_zat:.2f} mL**")

    # 3. Cari volume larutan
    elif sub_menu == "Cari volume larutan (mL)":
        persen = st.number_input(
            "Masukkan konsentrasi (% v/v)",
            min_value=0.0,
            format="%.4f"
        )
        volume_zat = st.number_input(
            "Masukkan volume zat terlarut (mL)",
            min_value=0.0,
            format="%.2f"
        )

        if st.button("Hitung Volume Larutan"):
            if persen == 0:
                st.error("Konsentrasi tidak boleh 0.")
            else:
                volume_total = (volume_zat * 100) / persen
                st.success(f"✅ Volume larutan = **{volume_total:.2f} mL**")

