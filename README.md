# PROJETO-DE-ORCAMENTO-.PY
import csv

# ==========================
# ORÇAMENTO DE ALUGUEL
# ==========================

class Imovel:
    def __init__(self, tipo):
        self.tipo = tipo
        self.valor_aluguel = 0


class Apartamento(Imovel):
    def __init__(self, quartos, garagem, possui_criancas):
        super().__init__("Apartamento")

        self.valor_aluguel = 700

        if quartos == 2:
            self.valor_aluguel += 200

        if garagem:
            self.valor_aluguel += 300

        self.aplicar_desconto(possui_criancas)

    def aplicar_desconto(self, possui_criancas):
        if not possui_criancas:
            desconto = self.valor_aluguel * 0.05
            self.valor_aluguel -= desconto
            print(f"Desconto aplicado: R$ {desconto:.2f}")

class Casa(Imovel):
    def __init__(self, quartos, garagem):
        super().__init__("Casa")

        self.valor_aluguel = 900

        if quartos == 2:
            self.valor_aluguel += 250

        if garagem:
            self.valor_aluguel += 300


class Estudio(Imovel):
    def __init__(self, vagas):
        super().__init__("Estudio")

        self.valor_aluguel = 1200

        if vagas >= 2:
            self.valor_aluguel += 250

            if vagas > 2:
                self.valor_aluguel += (vagas - 2) * 60


class Contrato:
    def __init__(self, parcelas):
        self.valor_contrato = 2000

        if parcelas < 1:
            parcelas = 1
        elif parcelas > 5:
            parcelas = 5

        self.parcelas = parcelas
        self.valor_parcela = self.valor_contrato / parcelas


# ==========================
# FUNÇÃO PARA GERAR CSV
# ==========================

def gerar_csv(valor_aluguel):
    with open("orcamento.csv", "w", newline="", encoding="utf-8") as arquivo:
        escritor = csv.writer(arquivo)

        escritor.writerow(["Parcela", "Valor do Aluguel"])

        for i in range(1, 13):
            escritor.writerow([i, f"R$ {valor_aluguel:.2f}"])

    print("\nArquivo 'orcamento.csv' gerado com sucesso!")


# ==========================
# PROGRAMA PRINCIPAL
# ==========================

print("=" * 40)
print("      SISTEMA R.M IMOBILIÁRIA")
print("=" * 40)

print("\nTipos de Imóveis:")
print("1 - Apartamento")
print("2 - Casa")
print("3 - Estudio")

opcao = int(input("\nEscolha uma opção: "))

# Apartamento
if opcao == 1:
    quartos = int(input("Quantidade de quartos (1 ou 2): "))
    garagem = input("Deseja garagem? (S/N): ").upper() == "S"
    criancas = input("Possui crianças? (S/N): ").upper() == "S"

    imovel = Apartamento(quartos, garagem, criancas)

# Casa
elif opcao == 2:
    quartos = int(input("Quantidade de quartos (1 ou 2): "))
    garagem = input("Deseja garagem? (S/N): ").upper() == "S"

    imovel = Casa(quartos, garagem)

# Estudio
elif opcao == 3:
    vagas = int(input("Quantidade de vagas de estacionamento: "))

    imovel = Estudio(vagas)

else:
    print("Opção inválida!")
    exit()

# Contrato
parcelas = int(input("\nParcelar contrato em quantas vezes (1 a 5)? "))
contrato = Contrato(parcelas)

# Exibição do orçamento
print("\n" + "=" * 40)
print("          ORÇAMENTO")
print("=" * 40)

print(f"Tipo do imóvel: {imovel.tipo}")
print(f"Valor mensal do aluguel: R$ {imovel.valor_aluguel:.2f}")

print(f"\nValor do contrato: R$ {contrato.valor_contrato:.2f}")
print(f"Parcelamento: {contrato.parcelas}x de R$ {contrato.valor_parcela:.2f}")

print("\nValor total do primeiro mês:")
print(f"R$ {imovel.valor_aluguel + contrato.valor_parcela:.2f}")

# Gerar arquivo CSV
gerar_csv(imovel.valor_aluguel)

print("\nObrigado por utilizar o sistema da R.M Imobiliária!")
