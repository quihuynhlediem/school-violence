# Secondary Victimization in Public Communication About School Violence

A computational linguistic and framing analysis of how Vietnamese public communication about school violence represents victims, and how that varies across news media, schools, government bodies and NGOs.

## Research questions

- **RQ1:** What forms of secondary victimization (victim blaming, attribution of responsibility, minimization of victim experiences, and others) are most prevalent in public communication about school violence?
- **RQ2:** How do the prevalence and form of secondary victimization vary across communication sources (news media, school announcements, NGO campaigns, government communication)?
- **RQ3:** What framing strategies are commonly used, and how are victims represented within these frames?

## Repository contents

| Path | Description |
|---|---|
| `dataset.csv` | Raw collected corpus. It is never edited by the code. |
| `data_cleaning.ipynb` | Cleaning notebook that turns the raw file into the corpus layer |
| `outputs/dataset_clean.csv` | Cleaned corpus, ready for analysis |
| `outputs/excluded_rows.csv` | Rows dropped during cleaning, with the reason for each |
| `outputs/cleaning_log.csv` | Every automatic change to a value (row, field, old value, new value, rule) |
| `outputs/review_flags.csv` | Rows kept but needing a human check |
| `outputs/doc_id_registry.csv` | Permanent URL → `doc_id` mapping, so IDs are never renumbered |

## Data

The corpus covers Vietnamese-language texts about violence involving students in Vietnamese schools (student–student, teacher–student, or online bullying). It targets about 100 documents for each of four source types: **News**, **School**, **Government** and **NGO**.

One row is one unique public communication. The same text republished at another URL counts once.

### Corpus layer (`outputs/dataset_clean.csv`)

| Field | Description |
|---|---|
| `doc_id` | Stable ID, format `SV0001` |
| `source_type` | One of `News`, `School`, `Government`, `NGO` |
| `source_name` | Canonical organization or outlet name |
| `source_url` | Original URL. Unique, with tracking parameters removed |
| `publication_date` | `YYYY-MM-DD`, between 2019-01-01 and the end of collection |
| `publication_year` | Derived from the date |
| `title` | Headline, on one line |
| `full_text` | Body text only, paragraphs separated by `\n`, at least 100 words |
| `word_count` | Words after Vietnamese word segmentation ([underthesea](https://github.com/undertheseanlp/underthesea)) |
| `collector` | Team member who collected the text. Empty while still unconfirmed |

The file is UTF-8 CSV, fully quoted, with all text in Unicode NFC.

## Data cleaning

The notebook does the following:

- Normalizes source types, canonical source names, URLs and dates (DD/MM/YYYY → ISO).
- Cleans body text: converts to NFC, removes English translations from bilingual posts, and strips "Xem thêm" / related-link lines, photo captions, image file names and emoji.
- Removes duplicates (same URL, or the same text at another URL), keeping the earliest publication.
- Excludes texts outside the study window or under 100 words.
- Runs validation checks against the dataset specification before saving.

All decisions a person makes, such as manual date fixes, collector names and thresholds, live in **section 1 (Configuration and manual fixes)** of the notebook. Edit them there and re-run the notebook from the top.

### Running it

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install pandas underthesea jupyter
jupyter notebook data_cleaning.ipynb
```

## Analysis plan

1. **Phase 1, corpus description** (after cleaning): composition by source type and collector, publication timeline, text length, incident coverage, lexical overview and keyness.
2. **Annotation:** coding of genre, incident linkage, violence type, perpetrator–victim relation, victim age, and secondary victimization indicators per the codebook.
3. **Phase 2, annotation description:** prevalence and intensity of each indicator by source type, indicator co-occurrence, frame distribution, and victim representation (quoted, named, active or passive).

## Team

Bảo Anh, Diễm Quí, Linh Chi, Trâm Anh, Minh Ngọc
