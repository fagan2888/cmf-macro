# Empirical Macro - macroeconomic course for CMF

Drafted by Evgeny Pogrebnyak e.pogrebnyak __ gmail com   
Created 11:00 16.11.2015  
Last edited 12:47 28.12.2015

[1. Введение: макроэкономическая статистика и работа с базами данных временных рядов](#1)  
  
2\. Анализ временных рядов c экономическими данными  
[2.1 Фильтры, снятие тренда и реальный деловой цикл](#2-1)  
[2.2. VAR-модели](#2-2)  
[2.3. Прогнозирование в реальном времени](#2-3)  
  
3\. Структурное эконометрическое моделирование 
[3.1. Моделирование отдельных макроэкономических показетелей](#3-1)  
[3.2. Модели с одновременными уравнениями](#3-2)  
  
[4. Калибровочные модели: DSGE](#4)  
  
5\. Платежный баланс и валютные курсы  
[5.1. Платежный баланс](#5.1)  
[5.2. Валютные курсы](#5.2)  
  
[6. ДКП](#6)  


###### 1
## Введение: макроэкономическая статистика и работа с базами данных временных рядов
```
1. Introduction, macroeconomic statistics, time series data sources

1.1. Objectives of macroeconomic research, areas of research, methods/controversies.

    Diebold, F.X., The Past, Present, and Future of Macroeconomic Forecasting, Journal of Economic Perspectives, 12, 1998, 175-192. 
    
    ------------------------------------------------------------------------------------
    КОММЕНТАРИЙ: можно дать как в начале курса, так и в конце, для обобщения материала
    
    ЗАДАНИЯ ПО ПОДГОТОВКЕ КУРСА: 
    (1.1.а) разработать домашнее задание для студентов по этой статье и как его проверять 
    (1.1.b) презентация по обзору курса (2-4 слайда), комменатрии по логике курса
    Оба эти блока вспомогательные, не первоочередные.
    ------------------------------------------------------------------------------------
    
1.2. Review of macroeconomic statistics frameworks:
    - IO tables + SNA
    - Flow of funds
    - Monetary balances 

    КОММЕНТАРИЙ: при подготовке заданий можно использовать материал курса прошлого года,
                 готов выложить или выслать отдельно (ЕП).
    Приоритеты: (1.2.а), (1.2.b) - обязательно в начале курса, 
                (1.2.c) - если будем давать денежно-кредитную политику и банки, можно в конце курса    
                
    ЗАДАНИЯ:
    (1.2.а) ознакомить студентов с основными балансовыми уровнениями МОБ и системы национальных счетов 
            -- три способа расчета ВВП
            -- межстрановые сравнения структуры ВВП
            -- как на макроданных отвечать на прикладные вопросы анализа экономической ситуации
    (1.2.b) кратко объяснить методологию составления статистике денежных потоков (включая связь с финансовым счетом СНС) и                дать упражнения по ним. За основу берется статистика США, 
            Возможные упражнения: 
            -- описать основные пропорции финансовой системы
            -- ответить на контрольные вопросы ("как изменилась задолженность домохозяйств перед банками",
            "каково изменение госдолга за период, кто его держатели", и т.д.)
            
            URL: http://www.federalreserve.gov/releases/z1/
                 http://www.cbr.ru/statistics/?PrtId=fafbs
                 
    (1.2.c) статистика банковской системы (ЦБ+банки), денежный обзор (можно использовать прошлогодний материал 
            с обновленной статистикой)
            
1.3. Time series data and seasonal adjustment
1.3.1. Time series data sources (FRED/Quandl + Russian statistics) 
    - https://research.stlouisfed.org/fred2/
    - https://www.quandl.com/
    - https://github.com/epogrebnyak/rosstat-kep-data

    ПРИОРИТЕТ: очень высокий, желательно, чтобы материал этого раздела апробировали не только ответственные за этот блок, 
               но и все участники разработки курса. Мы хотим, чтобы студенты могли на равне с Excel уверенно использовать
               "автоматические" зарбуежные источники временных рядов по экономической статистике в R/Python (FRED -
               обязательно, quandl - опционально), а также попробовать организовать работу с российскими данными на основе 
               собственной открытой базы данных (https://github.com/epogrebnyak/rosstat-kep-data с дополнениями), а также
               данными ЦБ. Для этого надо соответсвующий код погонять самим и доработать для студентов. 
               
    ЗАДАНИЯ:          
    (1.3.1.а) примеры использование API FRED/Quandl для получения данных в R/Python (код программ)
    (1.3.1.b) примеры использования rosstat-kep-data
              URL:  https://github.com/epogrebnyak/rosstat-kep-data/blob/master/example_use_data.py
    
    ЗАДАНИЯ ПО ДОРАБОТКЕ ДАЗЫ ДАННЫХ:  
    Доработка инфтерфейса / расширение наполнения rosstat-kep-data
    (1.3.1.c) написать в R интерфейс для получения данных rosstat-kep-data/output 
            -- аккуратное чтение трех CSV файлов в датафреймы (+ возможно обновлние CSV из интернет) 
            -- м.б. датафрейм из базы данных sqlite (реализовано на питоне)
            
    (1.3.1.d) расширение охвата импорта публикации 
              URL: https://github.com/epogrebnyak/rosstat-kep-data/issues/83
    
    (1.3.1.e) дополнительные временные ряды по российской и/или международной статистике (добавить в rosstat-kep-data)
      -- обработка данных по ценам на нефть (https://github.com/epogrebnyak/fx-oil/blob/master/eia.py) 
      -- данные ЦБ по платежному балансу
      -- данные ЦБ по банковской статистике
      
      
1.3.2. Seasonal adjustment (X11, Traumo/SEATS).

   ESS guidelines on seasonal adjustment
   URL: http://ec.europa.eu/eurostat/documents/3859598/6830795/KS-GQ-15-001-EN-N.pdf/d8f1e5f5-251b-4a69-93e3-079031b74bd3 

   Seasonal Adjustment Methodology at BLS
   URL: http://www.bls.gov/cpi/cpisahoma.htm

   Default procedure (for rosstat-kep-data): https://github.com/epogrebnyak/additional/tree/master/seasonality
   

   КОММЕНТАРИЙ: в этом разделе надо дать общее представление о снятии сезонности, возможном инстурментарии 
                и придумать задания (разбираются в классе и для самостоятельной работы). В приницпе, это 
                отдельная изолированная тема, приоритет невысокий. 
                Дополнительно - сейчас в rosstat-kep-data складываются исходные данные, далее надо представлять 
                данные со снятой сезонностью, предложена процедура/пакет в R, нужно оценить ее применимость.
                
   ЗАДАНИЯ:
   (1.3.2.а) презентация + примеры + домашнее задание по снятию сезонности
   (1.3.2.b) как снимать сезонность в rosstat-kep-data
   

```



## 2. Анализ временных рядов c экономическими данными
###### 2-1
## 2.1 Фильтры, снятие тренда и реальный деловой цикл
```		
2. Working with macroeconomic time series

2.1. Detrending/filtering and business cycle 

    Introduction to Macro Data. Karel Mertens, Cornell University
    URL: https://courses.cit.cornell.edu/econ614/introduction.pdf       

    Detrending and business cycle facts. Fabio Canova
    URL: http://apps.eui.eu/Personal/Canova/Articles/debucy.pdf
	
    Resuscitating real business cycles. Robert G. King, Sergio T. Rebelo. // 
    Handbook of Macroeconomics, Volume 1, Edited by J.B. Taylor and M. Woodford	
    URL: http://www.tau.ac.il/~yashiv/rbc_handbook.pdf
	
    James H. Stock, Mark W. Watson. Business cycle fluctuations in US macroeconomic time series
    URL: http://www.nber.org/papers/w6528.pdf
    
    ЗАДАНИЯ:
    (2.1.a)
    - упражнения по снятию тренда/фильтрам: 
      -- объяснить смысл процедуры (какую проблему решает, каким обарзом, какие ожидаемые резултаты)
      -- как реализуется с помощью программ
      -- код для исполнения на  фактических данных 
     (2.1.b)
     Составить таблицы корреляций трендов экономических переменных c лагом к ВВП  + их стандартные 
     отклонения (есть примеры таблицы дам ссылки, ЕП)
     - интерпертировать такие готовые таблицы
     - уметь составлять такие таблицы самим (воспроизвости аналог готовой таблицы на более новых данных)
     - составить экспериментальные таблицы по российским данным
     (2.1.с)
     - теория бизнес-цикла - основные положения
     - рассказать зачем мы снимали тренды, что видно на остатках от трендов и какая теория может остатки описывать
```    

###### 2-2
## 2.2. VAR-модели
```	
2.2. Vector autoregression (VAR) models

(*) Modeling the United States Economy. 
	URL: http://es.mathworks.com/help/econ/examples/modeling-the-united-states-economy.html
	(note: text on web site is confusing, this is not a DSGE model, this is a VAR aproximation 
	       used to compar

    ЗАДАНИЕ:
    (2.2.а)
    - эконометрика VAR моделей
    
    (2.2.b)
     - введение в методологию - что делают с помощью этого класса моделей, какие результаты, какие ограничения?
     - воспроизвести американскую VAR модель (ссылка выше)
     - при возможности - российский аналог 
     - классное и/или домашнее задание для студентов 
    
     (2.2.c) другие несложные примеры VAR моделей
     
    ОТВЕТСТВЕННЫЕ:
    (2.2.а) -  
    (2.2.b) -
    @kate_pyltsyna (?)
    @anastasiacherkashina (?)
    (просьба подтвердить)
    (2.2.c) -  
```
 
###### 2-3
## 2.3. Прогнозирование в реальном времени 
```
2.3. Nowcasting and coincident indicators

(*) James H. Stock, Mark W. Watson. A Probability Model of The Coincident Economic Indicators. 
    URL: http://www.nber.org/papers/w2772 [SW-NBER]

   ЗАДАНИЕ:
   (2.3.а)   
   - фильтр Калмана 
   (2.3.b)   
    - введение в постановку задачи - тип решаемой задачи, альтернативные подходы, ожидаемые результаты
    - применение фильтра Калмана (ненаблюдаемое состояние экономики), воспроизвести  [SW-NBER]
   (2.3.c) 
   - показать аналог на российских данных 
   (2.3.c) 
   - альтернативные подходы: метод главных компонент (PC), что-то еще может быть.
   
   ОТВЕТСТВЕННЫЕ:
   (2.3.а) -   
   (2.3.b) -  
   (2.3.c) -
   (2.3.c) - 
```

##3. Структурное эконометрическое моделирование 
###### 3-1
##3.1. Моделирование отдельных макроэкономических показетелей
```	
3. Estimated structural modelling 

3.1. Individual indicators

3.1.1. GDP components - household consumption 

    Идеи для иллюстрации: 
     - цикличность финансового поведения домохозяйств
     - сглаживание потребления во времени
     - ... ?
     
3.1.2. GDP components - investment

    Идеи для иллюстрации: 
    - факторы инвестиций (уровень цен, загрузка мощностей)
    - неравномерность инвестиций в течение года, снятие сезонности 
    - Tobin's q
    - ... ?

3.1.3. GDP components - government spending (?)

    Идеи для иллюстрации: 
    - оптимальный уровень государсвтенного долга / устойчивость долга
    - неравномерность госрасходов в течение года, пики в конце фискального года 
      (есть примеры по США и РФ, из материалов прошлого года)
    - ... ?

3.1.4.  Prices and inflation.
     
    Идеи для иллюстрации: 
    - моделирование инфляции на потребительском как функции от факторов/компонент 
    - ... ?
    
    КОММЕНТАРИЙ:
    
	По каждой теме:
	- графики индикаторов (США, РФ, другие страны) и связанных с ними ситуаций
	- что влияет на индикатор, какие теоретические или практические идеи по его 
	  объяснению и прогнозированию  
	- примеры выбранной модели, объясняющей эти индикаторы:
	  -- для разбора в классе, и/или
	  -- домашнее задание 
	- ссылки на литературу
	
     ОТВЕСТВЕННЫЕ:
     (3.1.1.а) - 
     (3.1.2.а) - 
     (3.1.3.а) -
     (3.1.4.а) -
     
```
###### 3-2
##3.2. Структурные модели с одновременными уравнениями

```
3.2. Large-scale models: Fair model - US Economy

    http://fairmodel.econ.yale.edu/mmm1.htm

    КОММЕНТАРИИ:
    1. Основная идея - показать возможности создания, оценки и прогнозов по модели, содержащей систему 
    одновременных уравнений.
    2. Предалагется за основу взять Fair model в EViews, американские данные 
    - воспроизвести Fair model в EViews с американскими данными
    - что и как можно показывать с помощью этой модели?
    - упрощенная модель в R, какие методы оценивания используются 
    - при возмоности - пример модели на российских данных
    
    ОТВЕТСТВЕННЫЙ:
    @kate_pyltsyna
    @anastasiacherkashina
    (просьба подтвердить)
    
    3. Если есть другие предложения по одновременным уровенения - привевсттуются.
    
```

###### 4
## 4. Калибровочные модели: DSGE

```	
4. Calibrated models: dynamic stochastic general equilibrium models (DSGE)

   - http://www3.eeg.uminho.pt/economia/nipe/summerschool2012/index_ficheiros/outline.pdf
   - http://www.econ.nyu.edu/user/gertlerm/jep.21.4.pdf

    Wieland, Volker,  Tobias Cwik, Gernot J. Müller, Sebastian Schmidt and Maik Wolters. A New comparative approach 
	to macroeconomic modeling and policy analysis. Journal of Economic Behavior and Organization, August 2012, 
	Vol. 83, 523-541 
(*) URL: http://www.macromodelbase.com/

    Applications -  Monetary and fiscal policy rules.  See reading list in 
    Monetary Policy: Theory and Practice - Kiel ASP - Volker Wieland 
    https://www.ifw-kiel.de/ausbildung/asp/outlines/paper/Wieland2014.pdf
```

Проблема: присланные модели и материалы пока не выстраиваются в стройный курс + архив материалов в старой группе Slack потерян, просьба повторить, что высылалось ранее - в отдельном канале в новой группе. 

Идея по освещению этого блока:
- рассказать про область применения рассматриваемых моделей, решаемые задачи, результаты и ограничения 
- отдельно про математический аппарат, способы решения моделей
- дать базовые модели RBC модели с аналитическим представлением и программным кодом (показывается их создание и использование), желтаельно использвоать открытые средства (Octave вместо Матлаба, Python)
- рассказать как создаются более сложные модели и их примеры (без детального разбора), какие новые гипотезы вводятся, как это усложняет решение моделей
- продемонстрировать возможности зрелых моделей, например на оснвое каталога macromodelbase.com - какие вопросы можно иллюстрировать, как это делать?

Также см. [файл по DSGE](/dsge.md)

## Платежный баланс и валютные курсы
###### 5-1
## 5.1. Платежный баланс 
```
5. Balance of payments and foreign exchange rates

5.1  Balance of payments (BOP)

   ------------------------------------------------------------------------------------
   ЗАДАНИЕ:
   (5.1.а) Изложить, что интересное и полезное можно кратко рассказать и показать про  
   - основные концепции статистики ПБ, связь ПБ с СНС, возможности его моделирования
   - различные формы представления ПБ, описательная статистика по российским данным
   - ПБ других страны + международные потоки капитала (США/Китай)
   - связь ПБ с СНС
   
   По этому пункты нужны:
   - тезисы слайдов
   - основные иллюстрации (графики, таблицу, ссылки на источники)
   - задания дял работы в классе или домашнее задание
   
   КОММЕНТАРИЙ:
   Основным результатом знакомства с платежным балансом может быть понимание следующего:
   - счет текущих операций + финансовый счет + изменение резервов = 0 
   - как из ПБ достается расчет "оттока каптала"
   - можно замоделировать экспорт и импорт 
   - потоки капитала хуже моделируются 
   - валютный курс балансирует ПБ, как это можно замоделировать

   ОТВЕТСТВЕННЫЕ (5.1.а):
   ...
   ------------------------------------------------------------------------------------
   
```

###### 5-2
## 5.2. Валютные курсы
```
5.2 Exchange rates
   
- Puzzles in foreign exchange rate forecasting 

(*) L Sarno. Viewpoint: Towards a solution to the puzzles in exchange rate economics: where do we stand?
    http://onlinelibrary.wiley.com/doi/10.1111/j.0008-4085.2005.00298.x/abstract
    
    TODO(АП): предложения как рассказать эту статью + какие можно дать задания 

- Review of methods:

(*) Barbara Rossi. Exchange Rate Predictability. February 14, 2013
    URL: http://crei.cat/people/rossi/Rossi_ExchangeRatePredictability_Feb_13.pdf
    
    TODO(АП): предложения как рассказать эту статью + какие можно дать задания
    TODO(АП): ссылка на пррисалнный обзорный материал.

- Rouble vs oil:

   ЕП: 1. сбор и обобобщение результатов с Алексеем Филипповым
       2. чистая выдача данных для моделирования (https://github.com/epogrebnyak/fx-oil/)
       3. обновление графиков 

   ------------------------------------------------------------------------------------

   КОММЕНТАРИИ:
   
        Рассматривается как дополнительная лекция в конце завершающей стадии курса. Для нее желательно
        раздать используемые данные заранее и дать простые задания по работе с ними/для ознакомления 
        с материалом лекции через выполнение заданий. Таким образом проще будет установить связь 
        между презентацией и практическими работами. 
	
    More topics:
	- трансмиссия на валютном рынке (слайд со схемой, может быть в начале лекции, чтобы с 
	  его помощью объяснять все остальное)
	      TODO(АП): драфт слайда от руки (фото) для обсуждения
	- валютные кризисы: 
	      ТODO(ЕП) - найти ссылку с описанием
	      
	По 1 слайду, если кто-то возьмется: 
	- роль доллара, особенности его динамики против других валют или commodities
	- резервные валюты, юань как резервная валюта
	- валютные режимы, валютное регулирование - нужна какая-нибудь обзорная статья МВФ
	
    ОТВЕТСТВЕННЫЕ (5.2.а) :
	- Писанов Александр (АП)
	- Алексей Филиппов (АФ)

   ------------------------------------------------------------------------------------
```

###### 6
## 6. Денежно-кредитная политика 
```
6. Monetary transmission and montery policy 

    ...
    
    --------------------

    КОММЕНТАРИЙ: 
    1. можно использовать занятия по статистике банковского сектора прошлого года + слайды
    по трансмиссии, но отдельного плана по этому пункту нет, хотя он фактически нужен при анализе 
    денежной политики в DSGE. Если кто-то возьмется за раздел (выскажет предложения по формирванию 
    содержания), я готов его включить и дорабаотывать (ЕП). 
    2. есть отдельный доп. кейс - модель банковских резервов (кто-то высказывал интерес к 
    моделированию банковского сектора), недавно сталкивались с этой задачей можно построить 
    небольшую регрессионную модель, в какой-то части курса ее рассказать.
    
    
    ОТВЕТСТВЕННЫЕ:
    
    --------------------
```


---------------------------------------------------------------------------   
Комментарии (10:49 16.11.2015):

Основные направления/блоки в курсе следующие: 
- **эконометрика временных рядов (п.2)** + VAR модели + оценка состояния экономики в текущем времени (nowcasting), применение - анализ бизнес-циклов
(комментарий: как я понимаю эконометрический аппарат уже прочитает Ярослав, может быть дать какие-то дополнительные задания/примеры. Есть два понятных кейса - VAR модель по США + разработка оперативных индикаторов цикла)

- **структурные эконометрические модели (п.3)**: моделирование отдельных показателей (компоненты ВВП - потребление, инвестиции; ИПЦ; другие показатели) и large-scale модели, применение - построение прогнозов 
(комментарий: есть пример публичной large-scale модели для CША - Fair model, она в соственной программной оболочке и в EViews есть, хорошо, если бы кто-то мог посмотреть и рассказать вариант EViews, дать упрощенную версию. по отдельным показателям/блокам подход предлагается такой - смотрим данные, смотрим краткой теорию  от чего в приницпе может зависить потребление, инвестиции и т.д., строим простую модель рассматриваемого показателя, чтобы оцфировать идеи из теориии)

- **"калибровочные" модели DSGE-типа (п.4)**, применение - анализ денежно-кредитной и фискальной политики
(комментарий: это пока самая слабая часть, я сам такие модели не строил, есть каталог таких моделей www.macromodelbase.com, по инструментарию - MATLAB/Octave + Dynare/IRIS, на факультете кто-то читал курс по DSGE на матметодах, может что-то слушал этот курс, работал с этими моделями) 

По данным я работаю над небольшой базой данных российской статистики, из котрой можно будет брать ряды. Это должно ускорить работу студентов над задачами, чтобы они ни сидели над сбором и выбором статистики сами, что может занимать много времени. Сейчас в базу данных склдывается часть публикации Росстата "Краткосрочные экономические показатели" (КЭП), неплохо конечно расширить за счет данных ЦБ (например, в KЭП нет ряда мировых цен нефти). Для иностранных данных (США, в основном) - quandl/FRED. **(п. 1.3.1)**  
Вспомогательная тема по статистике - снятие сезонности.**(п. 1.3.2)**

Что мы не делаем, на данном этапе, скорее всего:
- международная торговля/потоки капитала
- экономические кризисы и антикризинсая политика
- экономика пенсионных систем
