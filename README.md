# Analiza koszyka: reguły asocjacyjne (EconoShop Express)

Eksploracja danych, laboratorium 6. Dane syntetyczne: 7 968 paragonów sklepu osiedlowego z września i października 2026 r. (`dane/paragony.csv`, jeden wiersz = jeden produkt na paragonie).

| Plik | Co to jest |
|---|---|
| `koszyk.py` | funkcje: macierz koszyków, wsparcie, pewność, lift, Apriori, scenariusz zysku (TODO 1–3 do uzupełnienia) |
| `tests/test_koszyk.py` | testy (`pytest -q`); na start `7 failed`, po TODO 1–3 `7 passed` |
| `lab6_student.ipynb` | notatnik z zadaniami Z1–Z6 |
| `app.py` | dashboard Streamlit |
| `render.yaml` | wdrożenie na Render |
| `.devcontainer/` | konfiguracja GitHub Codespaces (Python 3.13 i biblioteki instalują się same) |
| `.github/workflows/testy.yml` | testy na GitHubie po każdym push |

## 1. Praca w GitHub Codespaces

1. **Code → Codespaces → Create codespace on main**; poczekaj na koniec instalacji.
2. W terminalu: `pytest -q`, uzupełnij TODO 1–3 w `koszyk.py`, aż zobaczysz `7 passed`.
3. Otwórz `lab6_student.ipynb` → **Select Kernel → Python 3.13** → uruchamiaj komórki po kolei (Shift + Enter).
4. `streamlit run app.py` → **Open in Browser** (albo zakładka PORTS → 8501).

## 2. Zapis zmian

Panel **Source Control** → opis → **Commit** → **Sync Changes** (albo `git add .`, `git commit -m "..."`, `git push`). Zakładka **Actions**: zielony znaczek = testy przechodzą.

## 3. Render

**New → Blueprint** → **Connect** przy tym repozytorium → **Deploy Blueprint**. Ręcznie: **New → Web Service**, Build `pip install -r requirements.txt`, Start `streamlit run app.py --server.port $PORT --server.address 0.0.0.0 --server.headless true`, zmienna `PYTHON_VERSION` = `3.13.5`. Usługa Free usypia po 15 min bez ruchu.

## 4. Rekomendacja dla kierownika sklepu

Makaron i sos pomidorowy: 207 paragonów, pewność makaron → sos 0,31, lift 5,97 (stabilnie: 6,77 we wrześniu i 5,34 w październiku). Ustawiamy sos obok makaronu bez rabatu: 207 z 664 klientów z makaronem i tak kupuje sos, więc rabat 20% dałby −273,80 zł w dwa miesiące, a ekspozycja około +48 zł przy zerowym koszcie. Pieluchy i chusteczki nawilżane: 108 paragonów, pewność pieluchy → chusteczki 0,31, lift 6,33 (6,29 i 6,35 w kolejnych miesiącach); stawiamy chusteczki na półce z pieluchami albo robimy zestaw „pieluchy + chusteczki” bez obniżki ceny. Nie polecam działań na regule węgiel drzewny → kiełbasa (lift 6,94 i 117 paragonów we wrześniu, ale w październiku tylko 2 paragony): to reguła sezonowa i w zimie nie zadziała. Nie polecam też reguły pieluchy → piwo jako całodziennej: lift 2,95 dotyczy tylko godzin 17–19, w pozostałych godzinach lift 0,97. Plan testu A/B: losowo wybieramy połowę sklepów, w których przez 4 tygodnie stoi sos obok makaronu (a chusteczki przy pieluchach), druga połowa bez zmian; porównujemy odsetek paragonów z makaronem, które mają też sos, i sprzedaż sosu na 100 paragonów, a zmianę wprowadzamy wszędzie tylko przy wyraźnie lepszym wyniku grupy testowej.

Aplikacja: https://analiza-koszyka.onrender.com
