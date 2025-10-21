# TPC5---Aplica-o-para-Gest-o-de-Alunos.
def criar_turma():
    return []

def inserir_aluno(turma):
    if turma is None:
        print("Não existe nenhuma turma criada. Crie uma turma primeiro.")
        return
    nome = input("Nome do aluno: ")
    id_ = input("ID do aluno: ")
    ids_existentes = [a[1] for a in turma]
    if id_ in ids_existentes:
        print("Já existe um aluno com essa identificação. Tente novamente.")
    else:
        valid = False
        while not valid:
            partes = input(
                "Introduza 3 notas (TPC, projeto e teste) seguidos entre 1 e 9: ")
            if len(partes) != 3:
                print("Por favor introduza exactamente 3 valores.")
            else:
                notas = []
                erro = False
                for p in partes:
                    try:
                        notas.append(float(p))
                    except Exception:
                        erro = True
                if erro:
                    print("Notas inválidas. Utilize números.")
                else:
                    valid = True
        aluno = (nome, id_, notas)
        turma.append(aluno)
        print("Aluno inserido com sucesso.")

def lista_turma(turma):
    if turma is None or len(turma) == 0:
        print("A turma está vazia.")
    else:
        print("Lista da Turma")
        idx = 1
        for aluno in turma:
            nome, id_, notas = aluno
            print(f"{idx:2d}. Nome: {nome}, ID: {id_}, Notas: {notas}")
            idx = idx + 1

def consultar_aluno_por_id(turma):
    if turma is None or len(turma) == 0:
        print("A turma está vazia.")
        return
    id_busca = input("ID do aluno a consultar: ")
    encontrado = False
    for aluno in turma:
        if aluno[1] == id_busca:
            nome, id_, notas = aluno
            print(f"Aluno encontrado: Nome: {nome}, ID: {id_}, Notas: {notas}")
            encontrado = True
    if not encontrado:
        print("Aluno não encontrado.")

def guardar_ficheiro(turma, nome_ficheiro):
    if turma is None:
        print("Não há nenhuma turma para guardar.")
        return
    try:
        with open(nome_ficheiro, 'w', encoding='utf-8') as f:
            i = 0
            while i < len(turma):
                aluno = turma[i]
                linha = aluno[0] + "|" + aluno[1] + "|" + ",".join(str(n) for n in aluno[2]) + "\n"
                f.write(linha)
                i = i + 1
        print(f"Turma guardada em '{nome_ficheiro}'.")
    except Exception as e:
        print("Erro ao guardar ficheiro:", e)

def carregar_ficheiro():
    nome_ficheiro = input("Nome do ficheiro para carregar (ex: turma.txt): ")
    if nome_ficheiro == "":
        nome_ficheiro = "turma.txt"
    try:
        with open(nome_ficheiro, 'r', encoding='utf-8') as f:
            linhas = f.readlines()
        nova_turma = []
        i = 0
        while i < len(linhas):
            linha = linhas[i].strip()
            i = i + 1
            if linha != "":
                partes = linha.split("|")
                if len(partes) == 3:
                    nome = partes[0]
                    id_ = partes[1]
                    notas_ = partes[2]
                    erro_conv = False
                    try:
                        notas = [float(x) for x in notas_.split(",")]
                    except Exception:
                        notas = []
                        erro_conv = True
                    if (not erro_conv) and len(notas) == 3:
                        nova_turma.append((nome, id_, notas))
        print(f"Turma carregada de '{nome_ficheiro}'.")
        return nova_turma
    except FileNotFoundError:
        print(f"Ficheiro '{nome_ficheiro}' não existe.")
        return None
    except Exception as e:
        print("Erro ao carregar ficheiro:", e)
        return None

def turma_com_5_alunos(turma):
    if turma is None:
        print("Nenhuma turma criada. Crie uma turma primeiro.")
        return
    demo = [
        ("Gonçalo Sousa", "A001", [15.0, 16.5, 14.0]),
        ("Bruno Costa", "A002", [12.0, 13.5, 14.0]),
        ("Carla Mendes", "A003", [18.0, 17.0, 19.0]),
        ("Diogo Ramos", "A004", [10.0, 11.5, 12.0]),
        ("Eva Pereira", "A005", [16.0, 15.5, 17.0]),
]
    for aluno in demo:
        turma.append(aluno)
    print("Turma com 5 alunos criada.")

def mostrar_menu(turma_atual, turmas):
    print("Menu (Gestão de Alunos)")
    print(f"Turma criada: {turmas.index(turma_atual)+1 if turma_atual in turmas else 'Nenhuma'}")
    print("1: Criar uma nova turma")
    print("2: Inserir um aluno na turma criada")
    print("3: Listar a turma criada")
    print("4: Consultar um aluno por id na turma criada")
    print("5: Turma com 5 alunos")
    print("8: Guardar a turma criada em ficheiro")
    print("9: Carregar uma nova turma de ficheiro")
    print("0: Sair da aplicação")

def main():
    turmas = []
    turma_atual = None
    sair = False
    while not sair:
        mostrar_menu(turma_atual, turmas)
        opc = input("Escolha uma opção: ")
        if opc == '1':
            turma_atual = criar_turma()
            turmas.append(turma_atual)
            print(f"Nova turma criada. Total de turmas: {len(turmas)}")
        elif opc == '2':
            inserir_aluno(turma_atual)
        elif opc == '3':
            lista_turma(turma_atual)
        elif opc == '4':
            consultar_aluno_por_id(turma_atual)
        elif opc == '5':
            turma_com_5_alunos(turma_atual)
        elif opc == '8':
            if turma_atual is None:
                print("Não há nenhuma turma para guardar.")
            else:
                ficheiro = input("Nome do ficheiro para guardar (ex: turma.txt): ")
                if ficheiro == "":
                    ficheiro = "turma.txt"
                guardar_ficheiro(turma_atual, ficheiro)
        elif opc == '9':
            nova = carregar_ficheiro()
            if nova is not None:
                turma_atual = nova
                turmas.append(turma_atual)
        elif opc == '0':
            print("Saiu da aplicação.")
            sair = True
        else:
            print("Opção inválida. Tente novamente.")

if __name__ == "__main__":
    main()
