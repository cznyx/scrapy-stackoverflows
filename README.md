## Stackoverflow Career (Job & Companies) scrapper

Quick n Dirty implementation with scrappy to scrap all jobs and companies on http://careers.stackoverflow.com/jobs

## Dependencies
- scrappy (https://github.com/scrapy/scrapy)
- scrappy-redis (https://github.com/darkrho/scrapy-redis/)
- MySQL-Python
- redis-cli & redis-server
- MySql-server

## How to run
```bash
➜  scrapy-stackoverflows git:(master) ✗ scrapy list
stackoverflowjob
stackoverflowcompany
```

Import company schema into MySQL:
```
mysql -u user -p'pass' -H host jobs < company.sql
```

```bash
➜  scrapy-stackoverflows git:(master) ✗ scrapy crawl stackoverflowjob -o test.json -t json
```

Crawled results are cached in Redis:
```
redis 127.0.0.1:6379> LRANGE 'stackoverflowcompany:items' 0 11
```

You can also deploy scrapy as a daemon, see #http://scrapyd.readthedocs.org/en/latest/

## Sample output
https://github.com/jayzeng/scrapy-stackoverflows/blob/master/jobs.json

## word frequency analyzer - simple implementation to analyze all positions on GitHub (https://jobs.github.com/positions)
```
jayzeng@Jays-iMac:~/Projects/jobboard (*)
> python word_freq_analyzer.py                                                                                                                                                                                      master [f8d9fc0] modified untracked
analyzer.py:33: UnicodeWarning: Unicode equal comparison failed to convert both arguments to Unicode - interpreting them as being unequal
   [(u'experience', 1150), (u'work', 686), (u'team', 629), (u'development', 512), (u'web', 474), (u'software', 443),
   (u'data', 380), (u'working', 368), (u'new', 365), (u'design', 309), (u'product', 272), (u'knowledge', 268),
   (u'skills', 267), (u'company', 264), (u'code', 255), (u'looking', 254), (u'help', 249), (u'systems', 244),
   (u'building', 235), (u'people', 231), (u'build', 230), (u'years', 221), (u'technology', 217), (u'applications', 209),
   (u'mobile', 208), (u'best', 205), (u'strong', 204), (u'technical', 203), (u'like', 202), (u'great', 201),
   (u'engineering', 192), (u'tools', 190), (u'environment', 188), (u'javascript', 188), (u'business', 178),
   (u'developer', 174), (u'projects', 169), (u'make', 167), (u'requirements', 167), (u'services', 166),
   (u'including', 163), (u'application', 162), (u'technologies', 161), (u'open', 160), (u'bloomberg', 158),
   (u'developers', 155), (u'engineer', 155), (u'one', 155), (u'management', 151), (u'platform', 150), (u'ability', 149),
   (u'system', 147), (u'using', 147), (u'understanding', 146), (u'use', 145), (u'ruby', 143), (u'high', 140), (u'job', 136), (u'join', 136),
   (u'get', 135), (u'products', 134), (u'source', 132), (u'user', 132), (u'role', 131), (u'support', 130), (u'time', 130),
   (u'developing', 126), (u'good', 124), (u'python', 123), (u'testing', 123), (u'benefits', 121), (u'engineers', 121),
   (u'every', 119), (u'performance', 119), (u'features', 118), (u'customers', 117), (u'world', 116), (u'programming', 115),
   (u'based', 114), (u'infrastructure', 113), (u'excellent', 112), (u'information', 111), (u'need', 111), (u'app', 110),
   (u'computer', 109), (u'android', 108), (u'plus', 108), (u'problems', 107), (u'twilio', 107), (u'communication', 106),
   (u'responsibilities', 105), (u'agile', 104), (u'provide', 104), (u'rails', 104), (u'what', 104), (u'part', 103),
   (u'highly', 101), (u'learn', 101), (u'growing', 99), (u'know', 99)]
```
![word frequency](freq_plot.png)

Or analyze a specific company
```
jayzeng@Jays-iMac:~/Projects/jobboard (*)
> python word_freq_analyzer.py github                                                                                                                                                                     master [003d82c] modified untracked
word_freq_analyzer.py:43: UnicodeWarning: Unicode equal comparison failed to convert both arguments to Unicode - interpreting them as being unequal
  tokens = [token.lower() for token in all_tokens if token not in stop_words and token.lower().isalpha()]
  [(u'github', 14), (u'technical', 8), (u'customer', 7), (u'customers', 7), (u'help', 5), (u'we', 5), (u'experience', 4),
   (u'prospects', 4), (u'account', 3), (u'answer', 3), (u'prospective', 3), (u'sales', 3), (u'software', 3), (u'assist', 2),
   (u'awesome', 2), (u'benefits', 2), (u'build', 2), (u'engineer', 2), (u'enterprise', 2), (u'guide', 2), (u'organizations', 2),
   (u'people', 2), (u'product', 2), (u'purchasing', 2), (u'questions', 2), (u'relationships', 2), (u'service', 2),
   (u'successful', 2), (u'support', 2), (u'tam', 2), (u'team', 2), (u'within', 2)]
```

## TODO
- Backoff and retry base upon (e.g: http status code)
- A thin web interface to manipulate jobs (schedule, cancel jobs etc)
- Add more spiders and crawlers to cover other job boards
- Deploy it as a real-time API that returns job JSON
- Basic machine learning with PredictionIO or Mahout
- Ability to answer basic questions like, what is the top 10 hot jobs in Seattle?

