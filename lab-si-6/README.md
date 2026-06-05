# Lab SI-6 – Porownanie lokalnych modeli LLM

## Konfiguracja testu

**Testowana strona:** https://pl.wikipedia.org/wiki/Sztuczna_inteligencja

**Prompt systemowy:**
```
Jesteś asystentem analizującym treść strony internetowej.
Twoje zadanie to napisanie krótkiego podsumowania WYŁĄCZNIE na podstawie dostarczonej treści strony.
Nie dodawaj żadnych informacji spoza podanego tekstu.
Jeśli odpowiedź nie wynika z dostarczonego kontekstu, napisz: Brak informacji w podanym kontekście.
Pomijaj elementy nawigacyjne, reklamy i stopki.
Odpowiadaj WYŁĄCZNIE w języku polskim w formacie markdown.
```

**Prompt użytkownika (fragment):**
```
Analizujesz stronę internetową zatytułowaną: Sztuczna inteligencja – Wikipedia
Poniżej znajduje się treść tej strony.
Napisz krótkie podsumowanie w języku polskim i formacie markdown,
korzystając WYŁĄCZNIE z poniższej treści. Nie używaj wiedzy spoza tego tekstu.
[...treść strony...]
```

---

## Wyniki porownania

| Model | Jakość polszczyzny (1-5) | Trafność podsumowania (1-5) | Długość odpowiedzi | Czas generowania | Uwagi |
|-------|:------------------------:|:---------------------------:|-------------------|-----------------|-------|
| llama3.2 | 4 | 3 | Krótka (2 zdania) | 5s | Jedyny który poprawnie podsumował stronę |
| qwen2.5:7b | 4 | 0 | Krótka | 30s | Zignorował zadanie, zadawał pytania zamiast podsumować |
| qwen2.5:3b | 3 | 1 | Bardzo długa | 35s | Próbował podsumować ale halucynował – wymyślał fakty spoza kontekstu |
| phi4-mini | 4 | 0 | Średnia | 12s | Zignorował zadanie, odpowiedział jak ogólny asystent AI |

---

## Odpowiedzi modeli

### llama3.2

```
Sztuczna inteligencja (ang. artificial intelligence, AI) – dyscyplina naukowa zajmująca się
rozwojem i zastosowaniami technologii umiejętności ludzkich. Termin ten został po raz pierwszy
użyty w 1956 roku przez angielskiego informatyka i filozofa Alanisa Turinga.
```

### qwen2.5:7b

```
Tema dodany! Czy jesteś zainteresowany okresem historii sztucznej inteligencji, jej typami,
zaletami i wady, albo może chcesz dowiedzieć się więcej na temat konkretnych aplikacji AI?
Podziel się swoimi interesami, a ja pomogę Ci znaleźć odpowiednie informacje.
```

### qwen2.5:3b

```
[Długa odpowiedź z kategoriami i strukturą, jednak zawierająca wymyślone fakty, np.:
"Albert Einstein przypominał sobie o pojęciach inteligencji maszynowej" –
informacji tej nie ma w kontekście. Model nie zastosował się do instrukcji
korzystania wyłącznie z podanej treści.]
```

### phi4-mini

```
Jestem tutaj, aby pomóc Ci. Jeśli masz jakieś pytania lub potrzebujesz informacji
na konkretny temat, śmiało pytaj! Działanie za pomocą AI może obejmować wiele różnych
funkcji, takich jak: generowanie treści, rozwiązywanie problemów, edukacja...
```

---

## Wnioski

- **llama3.2** jako jedyny model poprawnie wykonał zadanie – odpowiedział po polsku i na temat strony, mimo że jest najmniejszym modelem (2 GB). Odpowiedź była krótka, ale zgodna z instrukcją.
- **qwen2.5:7b** pomimo dobrej polszczyzny całkowicie zignorował instrukcję podsumowania – zachował się jak chatbot zadający pytania użytkownikowi zamiast analizować stronę.
- **qwen2.5:3b** podjął próbę podsumowania, ale zaczął halucynować – dodawał informacje spoza kontekstu (np. zmyślone cytaty Einsteina), co jest dokładnie tym, czego prompt zabraniał.
- **phi4-mini** zignorował cały kontekst i zadanie, odpowiadając jak generyczny asystent AI bez odniesienia do treści strony.
- Rozmiar modelu nie przekłada się na jakość wykonania instrukcji – llama3.2 (2 GB, 5s) wygrał z modelami 2–4 razy większymi.
- Większe modele (qwen2.5:3b, qwen2.5:7b) są wolniejsze (30–35s) i nie zawsze lepiej rozumieją złożone instrukcje systemowe.
- Kluczowym problemem okazało się **przestrzeganie instrukcji z promptu systemowego** – większość modeli je zignorowała.
