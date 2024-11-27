# Dinâmica: Detectives de Bugs Mutantes

## Descrição

Nesta dinâmica, os participantes assumirão o papel de "detectives de software" para identificar e eliminar bugs (mutantes) introduzidos em um código. Utilizando a ferramenta **MutPy**, os grupos irão rodar testes para identificar quais mutantes sobreviveram, melhorar os testes existentes e garantir que todos os mutantes sejam "mortos". A dinâmica será realizada no **VS Code**, utilizando Python em máquinas com sistema operacional Windows.

---

## Etapas da Dinâmica

### **1. Preparação do Ambiente**

1. **Instale o VS Code e a Extensão Python**
   - Baixe e instale o VS Code no site oficial: [https://code.visualstudio.com/](https://code.visualstudio.com/).
   - No VS Code, acesse a aba de extensões (`Ctrl+Shift+X`).
   - Pesquise por **Python** (Microsoft) e clique em **Instalar**.

2. **Instale o Python e o MutPy**
   - Baixe e instale o [Python 3](https://www.python.org/downloads/) no Windows.
   - Durante a instalação, marque a opção **"Add Python to PATH"**.
   - Abra o **Prompt de Comando** e instale o MutPy:
     ```bash
     pip install mutpy
     ```

3. **Configuração do Projeto**
   - Crie uma pasta chamada `detectives-dinamica` com a seguinte estrutura:
     ```
     detectives-dinamica/
     ├── src/
     │   └── math_operations.py    # Código mutável
     ├── tests/
     │   └── test_math_operations.py  # Testes iniciais
     ```

4. **Adicione os Arquivos**
   - **Código Base (`src/math_operations.py`)**:
     ```python
     def add(a, b):
         return a + b  # Mutante: return a - b
     ```
   - **Testes Iniciais (`tests/test_math_operations.py`)**:
     ```python
     import unittest
     from src.math_operations import add

     class TestMathOperations(unittest.TestCase):
         def test_add(self):
             self.assertEqual(add(2, 3), 5)  # Não cobre mutantes

     if __name__ == "__main__":
         unittest.main()
     ```

5. **Abra o Projeto no VS Code**
   - No VS Code, clique em **Arquivo > Abrir Pasta** e selecione a pasta do projeto.
   - Configure o interpretador Python:
     - Pressione `Ctrl+Shift+P`, digite **"Python: Select Interpreter"** e escolha o interpretador instalado.

---

### **2. Realização da Dinâmica**

#### **Passo 1: Identificar Mutantes Sobreviventes**
- No terminal integrado do VS Code (`Ctrl+``), execute o comando:
  ```bash
  mut.py --target src --unit-test tests
  ```
- Analise o relatório gerado, que mostrará os mutantes sobreviventes. Por exemplo:
  ```
  Mutant 1: survived
  Line 2: Replaced '+' with '-'
  ```

#### **Passo 2: Melhorar os Testes**
- Abra o arquivo `test_math_operations.py` e identifique os cenários não cobertos pelos testes.
- Adicione novos casos de teste para cobrir os mutantes sobreviventes.

**Exemplo de Teste Melhorado**:
```python
    def test_subtraction(self):
        self.assertNotEqual(add(2, 3), -1)  # Para cobrir o mutante
```

- Salve o arquivo e reexecute o MutPy para verificar se todos os mutantes foram mortos:
  ```bash
  mut.py --target src --unit-test tests
  ```

---

### **3. Apresentação dos Resultados**
- Cada grupo deve:
  - Mostrar os mutantes sobreviventes identificados no primeiro teste.
  - Explicar como melhoraram os testes para cobrir os cenários faltantes.
  - Demonstrar, rodando novamente o MutPy, que todos os mutantes foram eliminados.

---

## Objetivos da Dinâmica

- Compreender o funcionamento do Teste de Mutação.
- Identificar lacunas nos testes existentes e melhorar a cobertura de casos.
- Praticar o uso do **MutPy** e o **VS Code** no contexto de testes de software.

---

## Requisitos

- **Software**:
  - Python 3 instalado.
  - VS Code com a extensão Python.
  - MutPy instalado (`pip install mutpy`).
- **Estrutura do Projeto**:
  - Código base mutável (`src/math_operations.py`).
  - Testes iniciais incompletos (`tests/test_math_operations.py`).

---

## Exemplo de Relatório Gerado pelo MutPy

Quando o comando `mut.py` é executado, o relatório gerado mostrará:
- **Mutantes mortos**: Detecções bem-sucedidas pelos testes.
- **Mutantes sobreviventes**: Lacunas nos testes existentes.

Exemplo:
```
Mutation testing summary:
-------------------------
Total mutants: 3
Killed mutants: 1
Survived mutants: 2
```

---




---

### **1. Testes Básicos (Testes Iniciais)**
Estes testes são fornecidos inicialmente e não conseguem cobrir todos os casos, deixando mutantes sobreviverem.

```python
import unittest
from src.math_operations import add

class TestMathOperations(unittest.TestCase):
    def test_add(self):
        self.assertEqual(add(2, 3), 5)  # Testa soma básica
        self.assertEqual(add(0, 0), 0)  # Soma com zero

if __name__ == "__main__":
    unittest.main()
```

---

### **2. Testes Intermediários**
Estes testes são sugeridos quando os participantes identificarem que os mutantes sobrevivem devido à falta de cenários específicos, como somas negativas ou condições extremas.

```python
    def test_negative_numbers(self):
        self.assertEqual(add(-2, -3), -5)  # Testa soma com números negativos
        self.assertEqual(add(-2, 3), 1)    # Soma com negativo e positivo

    def test_large_numbers(self):
        self.assertEqual(add(1000000, 2000000), 3000000)  # Testa soma com números grandes
```

---

### **3. Testes Avançados**
Quando os participantes perceberem que mutantes ainda sobrevivem, ofereça estes testes avançados. Eles cobrem cenários mais detalhados, como resultados inesperados.

```python
    def test_not_equal(self):
        self.assertNotEqual(add(2, 3), -1)  # Garante que o resultado não é incorreto
        self.assertNotEqual(add(2, 3), 0)   # Evita mutantes que retornem constantes

    def test_edge_cases(self):
        self.assertEqual(add(1, -1), 0)     # Soma que resulta em zero
        self.assertEqual(add(0, 100), 100)  # Soma com um zero
```

---

### **4. Testes Extras (Para Desafios ou Diversão)**
Estes testes podem ser usados como desafio final para verificar se todos os mutantes foram cobertos.

```python
    def test_float_numbers(self):
        self.assertEqual(add(2.5, 3.5), 6.0)  # Soma com números de ponto flutuante

    def test_boolean_values(self):
        self.assertEqual(add(True, False), 1)  # Testa soma com valores booleanos

    def test_large_negative_numbers(self):
        self.assertEqual(add(-1000000, -2000000), -3000000)  # Testa soma de negativos grandes
```

---

### **Dicas para os Participantes**
1. Execute o **MutPy** após adicionar novos testes:
   ```bash
   mut.py --target src --unit-test tests
   ```
2. Analise o relatório para verificar quais mutantes sobreviveram e quais foram mortos.
3. Adapte os testes para cobrir os cenários restantes.
 ou explicações, me avise!
