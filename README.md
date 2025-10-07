# Corpus de Tweets com [SE AO MENOS] insubordinado em Português – Coleta de 2021 (Anonimizado)

*Resumo:* Corpus de tweets em português do Brasil desenvolvido para a investigação de mestrado em Linguística sobre construções condicionais insubordinadas com *[SE AO MENOS]*. Arquivo principal em formato *JSON*. Projeto financiado pela *FAPESP*; dissertação disponível no repositório da *UFSCar*.

---

# Sumário
- Descrição
- Justificativa e objetivo da pesquisa
- Metodologia de coleta (resumo)
- Anonimização e privacidade
- Formato dos dados (campos e exemplos)
- Como usar (exemplos em Python / Colab)
- Limitações e vieses
- Ética e anonimização
- Como citar
- Acesso ao trabalho completo
- Licença
- Agradecimentos
- Contato

---

# Descrição
Este repositório contém um corpus de tweets (coleta: 2021) anonimizados e preparados para análise linguística quanto ao uso da construção condicional insubordinada encabeçada por *[se ao menos]* no português do Brasil. O arquivo fornecido é corpus_tweets.json (lista de objetos JSON), pensado para integração imediata em pipelines de PLN e para reprodução de experimentos futuros, para além da dissertação desenvolvida.

---

# Justificativa e objetivo da pesquisa
O objetivo principal é investigar os padrões morfossintáticos e diferentes valores semântico-pragmáticos da construção condicional insubordinada com *[se ao menos]* e como ela aparece nas redes sociais, especificamente no Twitter. Tweets são uma fonte importante de dados espontâneos e de uso coloquial, tornando-os úteis para tais estudos de construções insubordinadas. Além disso, o trabalho buscou propor uma tipologia das construções condicionais insubordinadas com [SE AO MENOS], baseada no trabalho de D'Hertefelt (2015). Para maiores detalhes, consultar a dissertação disponível no Repositório Institucional da Universidade Federal de São Carlos (https://repositorio.ufscar.br/handle/20.500.14289/22841).

---

# Metodologia de coleta (resumo)

- *Período de coleta:* 2021 (agosto/2021 até novembro/2021).
- *Critérios de seleção:* tweets em português contendo a construção [SE AO MENOS] de modo insubordinado. Foram selecionadas 4 coletas de 4 meses distintos para proporcionar um corpus diverso.
- *Ferramentas:* utilização do recurso presente no site Vicinitas (atualmente indisponível). O site permitia coletas com base em uma palavra-chave, no caso, [SE AO MENOS].
- *Filtragem:* remoção de retweets e duplicatas quando aplicável; remoção dos usos subordinados; anonimização de usuários mencionados; remoção de links e demais recursos que poderiam causar problemas na análise.

---

# Anonimização e privacidade
Para garantir privacidade e reduzir risco de identificação:
- *Menções de usuários* substituídas por [user].
- *URLs* substituídas por [link].
- *IDs de usuários* e identificadores diretos foram removidos.
- *Metadados sensíveis* (quando presentes) foram excluídos.
- Não são fornecidos campos que permitam reidentificação direta (ex.: user_id, screen_name).
- *Aviso:* mesmo com anonimização, frases textuais podem ser pesquisáveis em motores externos; use com responsabilidade.

---

# Formato dos dados — campos e exemplo

*Arquivo principal:* corpus_tweets.json  
*Formato:* JSON — lista de objetos. Cada objeto representa um tweet.

*Campos presentes no arquivo:*
- id (string) — identificador interno (p. ex. "001")  
- texto (string) — conteúdo do tweet (com [user] e [link] quando aplicável)  
- menções (int) — número de ocorrências de [user]  
- links (int) — número de ocorrências de [link]  
- ano (int) — 2021  
- idioma (string) — pt-BR  
- fonte (string) — "Twitter"  
- tokens (int) — contagem simples de palavras (estimativa)  
- comentários (string) — observações curtas (opcional)

*Exemplo de objeto JSON:*
```json
{
  "id": "001",
  "texto": "Se ao menos o [user] estivesse alertando ao povo isso todo dia no último ano! Péssimo deputado! Não tem mais o meu voto111 [link]",
  "menções": 1,
  "links": 1,
  "ano": 2021,
  "idioma": "pt-BR",
  "fonte": "Twitter",
  "tokens": 25,
  "comentários": "Tweet político; crítica a figura pública; contém menção e link."
}

```
## Como usar (exemplos rápidos)

### Carregar o JSON em Python

```python
import json

with open("corpus_tweets.json", "r", encoding="utf-8") as f:
    corpus = json.load(f)

# exemplo: contar quantos tweets têm "se ao menos"
ocorrencias = [t for t in corpus if "se ao menos" in t["texto"].lower()]
print("Ocorrências de 'se ao menos':", len(ocorrencias))
```

### Exportar apenas o conteúdo textual dos tweets
```python
import json

input_path = “endereço_do_corpus.json"
output_path = "endereço_e_nome_do_arquivo.txt"

with open(input_path, "r", encoding="utf-8") as f:
    corpus = json.load(f)

with open(output_path, "w", encoding="utf-8") as f:
    for tweet in corpus:
        id_ = tweet.get("id", "")  # usa 'sem_id' se não houver id
        texto = tweet.get("texto", "").strip().replace("\n", " ")  # evita quebra de linha
        f.write(f"{id_}\t{texto}\n")

print(f" Arquivo exportado com sucesso em: {output_path}")
```

### Exemplo no Google Colab: resumo estatístico do corpus
```python
import json
import statistics

Carregando o corpus (substitua pelo caminho do arquivo)
with open("/content/corpus_tweets.json", "r", encoding="utf-8") as f:
    corpus = json.load(f)

total = len(corpus)
total_com_links = sum(1 for t in corpus if t["links"] > 0)
total_com_mencoes = sum(1 for t in corpus if t["menções"] > 0)

print(f"Total de tweets: {total}")
print(f"Tweets com links: {total_com_links}")
print(f"Tweets com menções: {total_com_mencoes}")
```
### Buscar ocorrências relevantes com expressões regulares
```python
import re

pattern = re.compile(r"\bse ao menos\b", flags=re.IGNORECASE)
matches = [t for t in corpus if pattern.search(t["texto"])]

print(f"Total de ocorrências da expressão: {len(matches)}")
print("Exemplo de tweet:", matches[0]["texto"]
```

### Contar frequências simples de palavras (tokenização leve)
```python
from collections import Counter
import re

def tokenize(text):
    return re.findall(r"\b\w+\b", text.lower())

# criar lista de todos os tokens do corpus
tokens = []
for t in corpus:
    tokens.extend(tokenize(t["texto"]))

frequencias = Counter(tokens)
print(frequencias.most_common(10))  # 10 palavras mais frequentes
```
### Analisar coocorrências da construção [SE AO MENOS]

```python
from collections import defaultdict

coocorrencias = defaultdict(int)

for t in corpus:
    palavras = t["texto"].lower().split()
    # procura o padrão "se ao menos"
    for i in range(len(palavras) - 2):
        if palavras[i] == "se" and palavras[i+1] == "ao" and palavras[i+2] == "menos":
            janela = palavras[i+3:i+6]  # 3 palavras depois de "menos"
            for p in janela:
                coocorrencias[p] += 1

print("Palavras mais frequentes depois de 'se ao menos':")
print(sorted(coocorrencias.items(), key=lambda x: x[1], reverse=True)[:10])

```
### Exportar subconjunto filtrado (ex.: subconjunto de tweets políticos)
```python
import json

tweets_politicos = [t for t in corpus if "voto" in t["texto"].lower() or "deputado" in t["texto"].lower()]

with open("tweets_politicos.json", "w", encoding="utf-8") as f:
    json.dump(tweets_politicos, f, ensure_ascii=False, indent=2)

print(f"Arquivo salvo com {len(tweets_politicos)} tweets.")
```

## Limitações e vieses

Este corpus reflete o uso espontâneo da língua portuguesa em uma plataforma digital específica (Twitter) no ano de 2021.  
Por esse motivo, apresenta limitações inerentes a esse tipo de dado:

- *Recorte temporal:* os tweets foram coletados em um período delimitado.  
- *Recorte sociolinguístico:* a amostra pode privilegiar certos grupos de usuários mais ativos na rede.  
- *Ruído linguístico:* como todo dado de rede social, contém variações ortográficas, abreviações, ironias e desvios gramaticais.  
- *Contexto de coleta:* apesar da anonimização, o contexto de produção (debates políticos, pandemia, etc.) pode influenciar padrões de uso.  

Esses fatores devem ser considerados na interpretação dos resultados e na replicação de análises.

---

## Ética e anonimização

Todos os dados foram *anonimizados* antes da disponibilização pública.  
As menções a usuários e links foram substituídas por marcadores genéricos:

- [user] → substitui nomes de usuários, perfis ou menções diretas.  
- [link] → substitui URLs externas, imagens ou vídeos.  

Não há qualquer informação identificável sobre os autores originais dos tweets.  
A coleta e o tratamento dos dados seguiram os princípios éticos da pesquisa científica e respeitam a Política de Privacidade do Twitter vigente à época da coleta.

---

## Como citar

Se este corpus for utilizado total ou parcialmente em trabalhos acadêmicos, cite-o da seguinte forma:

> COMARIM, Maria Julia Bernardo. Corpus de tweets insubordinados com [se ao menos] no português do Brasil. 2025. FAPESP Processos nº 2023/08945-6 e 2024/08629-0 – Universidade Federal de São Carlos, São Carlos, 2025.

Você também pode incluir o link direto para este repositório no GitHub.

---

## Acesso ao trabalho completo

> COMARIM, Maria Julia Bernardo. A expressão de valores desiderativos em construções condicionais insubordinadas com [SE AO MENOS] sob um viés cognitivo construcional. 2025. Dissertação (Mestrado em Linguística) – Universidade Federal de São Carlos, São Carlos, 2025. Disponível em: https://repositorio.ufscar.br/handle/20.500.14289/22841

## Licença

Este corpus é disponibilizado sob a licença *Creative Commons Attribution 4.0 International (CC BY-NC 4.0)*.  
Isso significa que você pode *copiar, redistribuir, remixar, transformar e construir a partir do material, para fins não comerciais, desde que forneça **crédito adequado à autora*.

📘 Texto completo da licença: [CC BY-NC 4.0] https://creativecommons.org/licenses/by-nc/4.0/


---
## Agradecimentos

Este projeto foi desenvolvido como parte da pesquisa de mestrado de *Maria Julia Bernardo Comarim* no Programa de Pós-Graduação em Linguística da *Universidade Federal de São Carlos (UFSCar)*, com financiamento da *Fundação de Amparo à Pesquisa do Estado de São Paulo* (FAPESP/ Processos nº 2023/08945-6 e 2024/08629-0), sob orientação da Profa. Dra. Flávia Bezerra de Menezes Hirata-Vale. 

---

## Contato

Para dúvidas, sugestões ou colaborações, entre em contato:

*Maria Julia Bernardo Comarim*

GitHub: @mjbcomarim https://github.com/mjbcomarim

Lattes: http://lattes.cnpq.br/7592091027326346

---
