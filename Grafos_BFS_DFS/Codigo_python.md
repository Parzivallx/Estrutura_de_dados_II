"""
# 📘 Estrutura de Dados II

## 🌐 Grafos, BFS e DFS — Simulação de Conectividade entre Cidades

**Objetivo:** estudar a conectividade entre cidades fictícias utilizando grafos,
comparando os algoritmos de busca em largura (BFS) e busca em profundidade (DFS).

> ⚠️ **Aviso:** Esta é uma simulação didática. Não representa um modelo de
> navegação aeronáutica e não deve ser utilizada para orientar voos reais.

## 🧭 Pergunta de pesquisa

Quantas conexões (arestas) podem ser bloqueadas na rede simulada antes que
deixe de existir um caminho entre Brasília e Curitiba, e como BFS e DFS se
comportam em cada cenário?

## 💻 Como executar

python solucao_grafos.py            -> menu interativo
python solucao_grafos.py --testes   -> executa os testes automatizados
"""

import sys
import unicodedata
from collections import deque


# ---------------------------------------------------------------------------
# 🗺️ 1. MODELAGEM DO GRAFO
# ---------------------------------------------------------------------------
# Grafo não direcionado, representado por uma lista de adjacência.
# Os vértices e as conexões são fictícios e servem apenas ao estudo
# dos algoritmos de busca e conectividade.

GRAFO = {
    "Brasília":       ["Goiânia", "Belo Horizonte", "Uberlândia"],
    "Goiânia":        ["Brasília", "Uberlândia", "Campo Grande"],
    "Uberlândia":     ["Brasília", "Goiânia", "Belo Horizonte", "Campinas"],
    "Belo Horizonte": ["Brasília", "Uberlândia", "Rio de Janeiro"],
    "Rio de Janeiro": ["Belo Horizonte", "São Paulo"],
    "São Paulo":      ["Rio de Janeiro", "Campinas", "Campo Grande", "Curitiba"],
    "Campinas":       ["Uberlândia", "São Paulo"],
    "Campo Grande":   ["Goiânia", "São Paulo", "Curitiba"],
    "Curitiba":       ["São Paulo", "Campo Grande", "Florianópolis"],
    "Florianópolis":  ["Curitiba"],
}


# ---------------------------------------------------------------------------
# 🔍 1.1. VALIDAÇÃO DA ESTRUTURA DO GRAFO
# ---------------------------------------------------------------------------

def validar_grafo(rede):
    """Confere a simetria do grafo não direcionado."""
    for a, vizinhos in rede.items():
        for b in vizinhos:
            if a not in rede.get(b, []):
                raise ValueError(f"Aresta assimétrica: {a} -> {b}")


# ---------------------------------------------------------------------------
# ⚙️ 2. IMPLEMENTAÇÃO DOS ALGORITMOS
# ---------------------------------------------------------------------------

# 🔵 2.1. BFS — Busca em largura
# Utiliza uma fila FIFO e encontra o menor caminho em quantidade de arestas
# em grafos não ponderados.

def bfs(rede, origem, destino):
    """Busca em largura. Retorna o menor caminho ou None."""
    fila = deque([(origem, [origem])])
    visitados = {origem}

    while fila:
        atual, caminho = fila.popleft()

        if atual == destino:
            return caminho

        for vizinho in rede.get(atual, []):
            if vizinho not in visitados:
                visitados.add(vizinho)
                fila.append((vizinho, caminho + [vizinho]))

    return None


# 🔎 2.2. DFS — Busca em profundidade
# Utiliza uma pilha LIFO e encontra um caminho possível, mas não garante
# o menor caminho. O resultado depende da ordem dos vizinhos.

def dfs(rede, origem, destino):
    """Busca em profundidade. Retorna um caminho ou None."""
    pilha = [(origem, [origem])]
    visitados = set()

    while pilha:
        atual, caminho = pilha.pop()

        if atual in visitados:
            continue

        visitados.add(atual)

        if atual == destino:
            return caminho

        for vizinho in reversed(rede.get(atual, [])):
            if vizinho not in visitados:
                pilha.append((vizinho, caminho + [vizinho]))

    return None


# ---------------------------------------------------------------------------
# 🔧 2.3. BLOQUEIO DE CONEXÕES
# ---------------------------------------------------------------------------

def bloquear_conexao(rede, a, b):
    """
    Devolve uma cópia da rede sem a conexão entre A e B.
    Remove a aresta nos dois sentidos e preserva o grafo original.
    """
    copia = {no: vizinhos.copy() for no, vizinhos in rede.items()}

    if b in copia.get(a, []):
        copia[a].remove(b)

    if a in copia.get(b, []):
        copia[b].remove(a)

    return copia


# ---------------------------------------------------------------------------
# 📊 2.4. FUNÇÕES AUXILIARES
# ---------------------------------------------------------------------------

def arestas_do_grafo(rede):
    """Lista as arestas únicas, sem duplicar A-B e B-A."""
    return sorted({
        tuple(sorted((a, b)))
        for a, vs in rede.items()
        for b in vs
    })


def contar_arestas(caminho):
    """Conta a quantidade de arestas de um caminho."""
    return len(caminho) - 1


# ---------------------------------------------------------------------------
# 🧭 3. ENTRADA DO USUÁRIO E VALIDAÇÃO DAS CIDADES
# ---------------------------------------------------------------------------

def _norm(texto):
    """Remove acentos, diferenças entre maiúsculas e minúsculas e espaços."""
    t = unicodedata.normalize("NFD", texto.strip())
    return "".join(
        c for c in t
        if unicodedata.category(c) != "Mn"
    ).lower()


def resolver_cidade(rede, texto):
    """Retorna o nome oficial da cidade ou None se não existir."""
    alvo = _norm(texto)

    for cidade in rede:
        if _norm(cidade) == alvo:
            return cidade

    return None


def pedir_cidade(rede, rotulo):
    """Solicita uma cidade ao usuário até receber uma opção válida."""
    while True:
        try:
            texto = input(f"{rotulo}: ")
        except EOFError:
            return None

        cidade = resolver_cidade(rede, texto)

        if cidade:
            return cidade

        print(
            f"  ✗ '{texto}' não existe na rede. "
            f"Opções: {', '.join(rede)}"
        )


# ---------------------------------------------------------------------------
# 📋 4. APRESENTAÇÃO E COMPARAÇÃO DOS RESULTADOS
# ---------------------------------------------------------------------------

def formatar(caminho):
    """Formata o caminho encontrado ou informa que não existe caminho."""
    if caminho is None:
        return "SEM CAMINHO (destino inalcançável neste cenário)"

    return (
        f"{' -> '.join(caminho)}  "
        f"[{contar_arestas(caminho)} arestas]"
    )


def comparar(rede, origem, destino, titulo):
    """Executa BFS e DFS e apresenta a comparação dos resultados."""
    print(f"\n=== {titulo} ===")
    print(f"Origem: {origem} | Destino: {destino}")

    c_bfs = bfs(rede, origem, destino)
    c_dfs = dfs(rede, origem, destino)

    print("BFS:", formatar(c_bfs))
    print("DFS:", formatar(c_dfs))

    if c_bfs and c_dfs:
        d = contar_arestas(c_dfs) - contar_arestas(c_bfs)

        if d == 0:
            print("Comparação: ambos usaram o mesmo número de arestas.")
        else:
            print(
                f"Comparação: DFS usou {d} aresta(s) a mais "
                "que BFS (BFS = mínimo)."
            )

    elif c_bfs is None and c_dfs is None:
        print(
            "Comparação: ambos concordam que não há caminho "
            "(conectividade perdida)."
        )

    return c_bfs, c_dfs


# ---------------------------------------------------------------------------
# 💻 5. MENU INTERATIVO
# ---------------------------------------------------------------------------

def menu():
    """Executa o menu principal da simulação."""
    validar_grafo(GRAFO)

    print(
        "Simulação didática de conectividade "
        "(NÃO é navegação aeronáutica real)."
    )
    print("Cidades:", ", ".join(GRAFO))

    # Lista de conexões bloqueadas.
    # O grafo original permanece intacto.
    bloqueios = []

    while True:
        print(
            "\n1) Buscar caminho  2) Bloquear conexão  "
            "3) Limpar bloqueios  4) Ver conexões  0) Sair"
        )

        try:
            op = input("Opção: ").strip()
        except EOFError:
            break

        if op == "0":
            break

        # 🔍 5.1. Buscar caminho entre duas cidades
        elif op == "1":
            origem = pedir_cidade(GRAFO, "Origem")
            destino = pedir_cidade(GRAFO, "Destino")

            if origem is None or destino is None:
                break

            comparar(GRAFO, origem, destino, "Rede original")

            if bloqueios:
                rede = GRAFO

                for a, b in bloqueios:
                    rede = bloquear_conexao(rede, a, b)

                comparar(
                    rede,
                    origem,
                    destino,
                    "Rede com bloqueios: "
                    + ", ".join(
                        f"{a}–{b}" for a, b in bloqueios
                    )
                )

        # 🚫 5.2. Bloquear uma conexão
        elif op == "2":
            a = pedir_cidade(GRAFO, "Cidade A")
            b = pedir_cidade(GRAFO, "Cidade B")

            if a is None or b is None:
                break

            if b not in GRAFO[a]:
                print(f"  ✗ Não existe conexão direta {a}–{b}.")

            elif (a, b) in bloqueios or (b, a) in bloqueios:
                print("  ! Essa conexão já está bloqueada.")

            else:
                bloqueios.append((a, b))
                print(
                    f"  ✓ Conexão {a}–{b} bloqueada "
                    "(apenas na simulação)."
                )

        # 🔄 5.3. Limpar os bloqueios
        elif op == "3":
            bloqueios.clear()
            print("  ✓ Bloqueios removidos.")

        # 📋 5.4. Listar as conexões
        elif op == "4":
            for a, b in arestas_do_grafo(GRAFO):
                print(f"  {a} — {b}")

        else:
            print("  ✗ Opção inválida.")


# ---------------------------------------------------------------------------
# 🧪 6. TESTES AUTOMATIZADOS
# ---------------------------------------------------------------------------

def testes():
    """Executa os testes automatizados e apresenta as evidências."""
    validar_grafo(GRAFO)

    # Guarda uma cópia para verificar se o grafo original foi preservado.
    original = {k: v.copy() for k, v in GRAFO.items()}

    ok = 0

    def checar(nome, condicao):
        nonlocal ok

        print(f"[{'PASS' if condicao else 'FAIL'}] {nome}")

        assert condicao, nome
        ok += 1

    O, D = "Brasília", "Curitiba"

    # -----------------------------------------------------------------------
    # 🛤️ 6.1. CENÁRIO A — REDE COMPLETA
    # -----------------------------------------------------------------------

    b, d = comparar(
        GRAFO, O, D,
        "Teste A - rede completa"
    )

    checar("A1: BFS acha caminho", b is not None)
    checar("A2: DFS acha caminho", d is not None)

    checar(
        "A3: BFS tem o menor nº de arestas (<= DFS)",
        contar_arestas(b) <= contar_arestas(d)
    )

    checar(
        "A4: caminhos começam na origem e terminam no destino",
        b[0] == O and b[-1] == D and d[0] == O and d[-1] == D
    )

    checar(
        "A5: BFS = 3 arestas (mínimo esperado)",
        contar_arestas(b) == 3
    )

    # -----------------------------------------------------------------------
    # 🚫 6.2. CENÁRIO B — BLOQUEIO UBERLÂNDIA–CAMPINAS
    # -----------------------------------------------------------------------

    rb = bloquear_conexao(
        GRAFO,
        "Uberlândia",
        "Campinas"
    )

    b, d = comparar(
        rb, O, D,
        "Teste B - bloqueio Uberlândia–Campinas"
    )

    checar(
        "B1: ainda existe caminho (rota alternativa)",
        b is not None and d is not None
    )

    checar(
        "B2: rede original NÃO foi alterada",
        GRAFO == original
    )

    checar(
        "B3: aresta removida nos dois sentidos",
        "Campinas" not in rb["Uberlândia"]
        and "Uberlândia" not in rb["Campinas"]
    )

    # -----------------------------------------------------------------------
    # 🚫 6.3. CENÁRIO C — BLOQUEIO GOIÂNIA–CAMPO GRANDE
    # -----------------------------------------------------------------------

    rc = bloquear_conexao(
        rb,
        "Goiânia",
        "Campo Grande"
    )

    b, d = comparar(
        rc, O, D,
        "Teste C - + bloqueio Goiânia–Campo Grande"
    )

    checar(
        "C1: ainda há caminho via Belo Horizonte–Rio de Janeiro–São Paulo",
        b is not None and d is not None
    )

    # -----------------------------------------------------------------------
    # ⚠️ 6.4. CENÁRIO C2 — TERCEIRO BLOQUEIO E REDE DIVIDIDA
    # -----------------------------------------------------------------------

    rc2 = bloquear_conexao(
        rc,
        "Rio de Janeiro",
        "São Paulo"
    )

    b, d = comparar(
        rc2, O, D,
        "Teste C2 - + bloqueio Rio de Janeiro–São Paulo (rede dividida)"
    )

    checar(
        "C2: sem caminho (BFS e DFS retornam None)",
        b is None and d is None
    )

    # -----------------------------------------------------------------------
    # 🔄 6.5. CENÁRIO D — ALTERAÇÃO DA ORDEM DOS VIZINHOS
    # -----------------------------------------------------------------------

    alterado = {
        k: v.copy()
        for k, v in GRAFO.items()
    }

    alterado["Brasília"].reverse()

    d1 = dfs(GRAFO, O, D)
    d2 = dfs(alterado, O, D)

    print(
        f"\n=== Teste D - ordem dos vizinhos ===\n"
        f"DFS ordem original: {formatar(d1)}"
        f"\nDFS ordem invertida: {formatar(d2)}"
    )

    checar(
        "D1: DFS muda com a ordem (mesmo grafo)",
        d1 != d2
    )

    checar(
        "D2: BFS mantém o nº mínimo de arestas",
        contar_arestas(bfs(GRAFO, O, D))
        == contar_arestas(bfs(alterado, O, D))
    )

    # -----------------------------------------------------------------------
    # 🧩 6.6. CENÁRIO E — CASOS DE BORDA E VALIDAÇÕES
    # -----------------------------------------------------------------------

    checar(
        "E1: origem == destino -> caminho de 1 vértice, 0 arestas",
        bfs(GRAFO, O, O) == [O]
        and dfs(GRAFO, O, O) == [O]
    )

    checar(
        "E2: cidade inexistente retorna None, sem erro",
        bfs(GRAFO, O, "Atlântida") is None
        and dfs(GRAFO, "Atlântida", D) is None
    )

    checar(
        "E3: validação ignora acento/caixa ('curitiba', 'brasilia')",
        resolver_cidade(GRAFO, "curitiba") == "Curitiba"
        and resolver_cidade(GRAFO, " BRASILIA ") == "Brasília"
    )

    checar(
        "E4: nome inexistente é rejeitado",
        resolver_cidade(GRAFO, "Atlântida") is None
    )

    rf = bloquear_conexao(
        GRAFO,
        "Curitiba",
        "Florianópolis"
    )

    checar(
        "E5: Florianópolis isolada -> sem caminho",
        bfs(rf, O, "Florianópolis") is None
        and dfs(rf, O, "Florianópolis") is None
    )

    checar(
        "E6: bloquear aresta inexistente não quebra nada",
        bloquear_conexao(
            GRAFO,
            "Brasília",
            "Curitiba"
        ) == GRAFO
    )

    checar(
        "E7: rede original intacta ao final",
        GRAFO == original
    )

    # -----------------------------------------------------------------------
    # 📊 6.7. RESUMO DOS TESTES
    # -----------------------------------------------------------------------

    print(f"\n{ok} verificações executadas com sucesso.")


# ---------------------------------------------------------------------------
# 🚀 7. EXECUÇÃO PRINCIPAL DO PROGRAMA
# ---------------------------------------------------------------------------

if __name__ == "__main__":
    if "--testes" in sys.argv:
        testes()
    else:
        menu()
