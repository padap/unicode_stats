# Unicode Stats

Быстрый анализ unicode символов

Цель: быстро детектировать галюцинации по типу иероглифов, украинского языка в различных бенчмарках

Быстрый и удобный wrapper к https://www.unicode.org/Public/UNIDATA/Blocks.txt 

## Использование
### Достать блоки юникода / детектировать язык
```python
from unicode_stats import unicode_block_parser

# Get all symbols
example_text = 'краї́中land'
print(unicode_block_parser.get_stats(example_text))
# > {'Cyrillic (Russian)': {'n': 3, 'symbols': 'арк'}, 'Cyrillic (Ukranian)': {'n': 1, 'symbols': 'ї'}, 'Combining Diacritical Marks': {'n': 1, 'symbols': '́'}, 'CJK Unified Ideographs': {'n': 1, 'symbols': '中'}, 'Basic Latin': {'n': 4, 'symbols': 'lnda'}}

# Get main language
example_text = 'краї́'
print(unicode_block_parser.get_lang(example_text))
# > Cyrillic (Ukranian)

# Get all languages
example_text = 'краї́'
print(unicode_block_parser.get_lang(example_text, return_main_lang=False))
# > ru,Cyrillic (Ukranian),Combining Diacritical Marks

unicode_block_parser.get_single_block("х")
# > Cyrillic (Russian)
```

### Создать статистику для jsonl файла

**Python**

```python
from unicode_stats.aggregation import AggregatedUnicodeBlockParser
agregated_parser = AggregatedUnicodeBlockParser(columns = "qwen", max_lines=1)
agregated_parser.get_stats("3model_cp.jsonl")
```

|    | block   | column   |    n |   rate | symbols   |   n_symbols | rows   | example_first   | example_last   |
|---:|:--------|:---------|-----:|-------:|:----------|------------:|:-------|:----------------|:---------------|
|  0 | Cyrilic (Russian)   | qwen     | 2161 |  0.777 | оеаи      |       57234 | [0, 1  | Чтобы           | Для р          |
|  1 | Basic   | qwen     | 2778 |  1     | оеаи      |       57234 | [0, 1  | Чтобы           | Для р          |

**Bash**
```bash
unicode_stats 3model_cp.jsonl --columns="qwen"
```
> Aggregated statistics saved to 3model_cp.csv 

## Установка
```bash
pip install dist/unicode_stats-{version}-py3-none-any.whl
```

## Сбор из исходников 
```bash
python -m build
```

Если тесты не пройдутся, пакет не соберется
## Запуск тестов
```bash
pytest
