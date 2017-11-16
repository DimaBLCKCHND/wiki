# График эмиссии
<!-- toc -->

Вы можете найти скрипты для построяния модели и графика супплая в директории  **_programs/util_**

Расчет производится на основе информации находящийся в вашей текущий сборке (нужно собрать проект) в файле config.hpp

Для запуска этого скрипта введите:
```bash
 ./inflation_model > model.json
```

Скрипт в течении пары минут сгенерирует файл  _model.json_, который содержит информацию по супплаю на 10 лет. Далее вы можете построить график использую данную модель:
```bash
python3 inflation_plot.py model.json
```

Также в файле inflation_plot.py вы можете внести изменения и построить график по модели удобным вам образом.


**Заметка #1** У вас в системе должен быть установлен _matplotlib_ для python3.
Пример для macOS:
```bash
brew install homebrew/python/numpy --with-python3
```

```bash
brew install homebrew/python/matplotlib --with-python3
```

**Заметка #2** Если вы хотите изучить файл model.json, то мы рекомендуем вам использовать редактор Sublime Text

**Что внутри модели**

```bash
{"rvec":["929159090641","8360617424769","929159090641","8360617424769","197985103985","1780051544865","195077031513","1755693283617","179687790278","1615357001502"],"b":68585000,"s":"24303404786580"}
```

rvec показывает общее количество **Голос сатоши** созданные с момента генезиса:

- Curation rewards
- Vesting rewards balancing curation rewards
- Content rewards
- Vesting rewards balancing content rewards
- Producer rewards
- Vesting rewards balancing producer rewards
- Liquidity rewards
- Vesting rewards balancing liquidity rewards
- PoW rewards
- Vesting rewards balancing PoW rewards

**b** - это номер **блока**

**s** - это **общий** суплай

**Некоторые возможные источники неточностей, направления и расчетные относительные размеры этих эффектов:**
**

- Missed blocks not modeled (lowers STEEM supply, small)
- Miner queue length very approximately modeled (assumed to go to 100 during the first blocks and then stay there) (may lower or raise STEEM supply, very small)
- Creation / destruction of STEEM used to back SBD not modeled (moves STEEM supply in direction opposite to changes in dollar value of 1 STEEM, large)
- Interest paid to SBD not modeled (raises STEEM supply, medium)
- Lost / forgotten private keys / wallets and deliberate burning of STEEM not modeled (lowers STEEM supply, unknown but likely small)
- Possible bugs or mismatches with implementation (unknown)
