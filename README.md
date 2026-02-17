Sistema de Recomendação para E-commerce (Zé Delivery)

Visão geral:

Este projeto implementa um sistema de recomendação de produtos para um cenário de e-commerce, utilizando dados de interações implícitas entre usuários e produtos.
O objetivo é comparar diferentes abordagens de recomendação e avaliar o impacto de cada uma nas métricas de ranking, evoluindo de modelos simples até um modelo híbrido que combina sinais colaborativos e de conteúdo.

Problema de negócio:

Em plataformas de e-commerce, a recomendação de produtos é fundamental para:

- Aumentar conversão
- Melhorar a experiência do usuário
- Reduzir churn
- Aumentar ticket médio

O desafio é recomendar itens relevantes mesmo quando:

- O usuário tem pouco histórico
- O catálogo possui muitos produtos
- Os dados são implícitos (cliques, carrinho, compras)



Dataset:

O dataset simula o comportamento de usuários em um sistema de delivery.

Interações:

- Cada registro contém:
- user_id
- product_id
- interaction_type:
- view
- cart_add
- purchase
- interaction_time

As interações são tratadas como feedback implícito.

Produtos:

Cada produto possui:

- product_id
- product_name
- category
- price
- brand
- alcohol_content
- Abordagem

O projeto segue uma evolução incremental de modelos:

- Baseline de popularidade
- Baseline por categoria
- Modelo colaborativo (SVD)
- Modelo híbrido (colaborativo + conteúdo)

Pré-processamento:

As interações foram convertidas em um score implícito:

- Interação	Peso
- view	1
- cart_add	3
- purchase	6

O score final é a soma dos pesos por usuário e produto.

Metodologia de avaliação:

Os dados foram divididos por usuário em:

- 80% treino
- 20% teste

Métricas utilizadas:

- Recall@10 → capacidade de recuperar itens relevantes
- MAP@10 → qualidade do ranking das recomendações

Modelos testados:

1. Popularidade

- Recomenda os produtos mais populares globalmente.
- Função: baseline simples e fallback.

Resultado:

- Recall@10: 0.154
- MAP@10: 0.0357

2. Popularidade por Categoria

Recomenda produtos populares dentro da última categoria consumida pelo usuário.

Resultado:

- Recall@10: 0.098
- MAP@10: 0.0382
- Interpretação:
- Melhor ranking
- Menor capacidade de recuperação

Usuários consomem múltiplas categorias

3. Modelo colaborativo (SVD)

Utiliza fatoração de matriz para aprender padrões de consumo entre usuários e produtos.

Resultado:

- Recall@10: 0.200

- MAP@10: 0.0655

Interpretação:

- +30% de ganho em Recall
- Ranking mais relevante
- Captura preferências coletivas

4. Modelo híbrido (SVD + conteúdo)

Combina: Score colaborativo (SVD)

Similaridade de conteúdo (TF-IDF de nome, categoria e marca)

Fórmula:

- score_final = 0.7 * score_colaborativo + 0.3 * score_conteudo


Resultado:

- Recall@10: 0.232
- MAP@10: 0.0625
- Interpretação:
- Melhor desempenho geral
- Combina padrões de consumo e semântica dos produtos

Mais robusto para cold start

Comparação final: 

- Modelo	Recall@10	MAP@10
- Popularidade	0.154	0.0357
- Categoria	0.098	0.0382
- SVD Colaborativo	0.200	0.0655
- Híbrido	0.232	0.0625

Principais insights:

- Modelos baseados apenas em popularidade têm desempenho limitado.
- Categoria isolada não representa bem o comportamento do usuário.
- O modelo colaborativo captura padrões de consumo mais fortes.
- O modelo híbrido apresenta o melhor desempenho geral.

Tecnologias utilizadas

- Python
- Pandas
- NumPy
- Scikit-learn
- SciPy
- Matplotlib
