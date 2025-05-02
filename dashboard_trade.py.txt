import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt

# Dados simulados
data = {
    "Produto": [
        "Cimento 50kg", "Massa corrida", "Argamassa A1",
        "Tinta Branco 18L", "Esmalte Premium", "Rejunte Cinza 1kg"
    ],
    "Loja A": [120, 45, 75, 20, 30, 95],
    "Loja B": [80, 90, 110, 60, 10, 130],
    "Loja C": [200, 40, 95, 35, 5, 85],
    "Loja D": [150, 30, 60, 25, 8, 100]
}

# Criando DataFrame
df = pd.DataFrame(data)
df["Total Vendido"] = df[["Loja A", "Loja B", "Loja C", "Loja D"]].sum(axis=1)
df = df.sort_values("Total Vendido", ascending=False)
df["Classificacao ABC"] = ["A", "A", "B", "B", "C", "C"]

# Título
st.title("Dashboard de Sell-out por Produto")

# Exibir a tabela
st.subheader("Tabela de Vendas por Loja e Total")
st.dataframe(df, use_container_width=True)

# Gráfico de barras
st.subheader("Total Vendido por Produto")
fig, ax = plt.subplots()
ax.barh(df["Produto"], df["Total Vendido"], color="seagreen")
ax.set_xlabel("Total Vendido (Unidades)")
ax.set_ylabel("Produto")
ax.invert_yaxis()
st.pyplot(fig)

# Filtro por classificação ABC
st.subheader("Filtrar por Classificação ABC")
abc_filter = st.multiselect("Escolha a(s) classificações:", options=df["Classificacao ABC"].unique(), default=["A"])
filtered_df = df[df["Classificacao ABC"].isin(abc_filter)]
st.dataframe(filtered_df)
