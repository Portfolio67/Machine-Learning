# Machine-Learning

<!doctype html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Data Structures</title>
</head>
<body>
<p>`‌from statistics import mean<br/>
import matplotlib<br/>
matplotlib.use('Agg')<br/>
import matplotlib.pyplot as plt<br/>
from collections import deque<br/>
import os<br/>
import csv<br/>
import numpy as np </p>

<p>SCORES_CSV_PATH = &quot;./scores/scores.csv&quot;<br/>
SCORES_PNG_PATH = &quot;./scores/scores.png&quot;<br/>
SOLVED_CSV_PATH = &quot;./scores/solved.csv&quot;<br/>
SOLVED_PNG_PATH = &quot;./scores/solved.png&quot;<br/>
AVERAGE_SCORE_TO_SOLVE = 195<br/>
CONSECUTIVE_RUNS_TO_SOLVE = 100 </p>

<p>class ScoreLogger: </p>

<pre><code>def __init__(self, env_name):  
    self.scores = deque(maxlen=CONSECUTIVE_RUNS_TO_SOLVE)  
    self.env_name = env_name  

    if os.path.exists(SCORES_PNG_PATH):  
        os.remove(SCORES_PNG_PATH)  
    if os.path.exists(SCORES_CSV_PATH):  
        os.remove(SCORES_CSV_PATH)  

def add_score(self, score, run):  
    self._save_csv(SCORES_CSV_PATH, score)  
    #self._save_png(input_path=SCORES_CSV_PATH,  
    #               output_path=SCORES_PNG_PATH,  
    #               x_label=&quot;runs&quot;,  
    #               y_label=&quot;scores&quot;,  
    #               average_of_n_last=CONSECUTIVE_RUNS_TO_SOLVE,  
    #               show_goal=True,  
    #               show_trend=True,  
    #               show_legend=True)  
    self.scores.append(score)  
    mean_score = mean(self.scores)  
    print (&quot;Scores: (min: &quot; + str(min(self.scores)) + &quot;, avg: &quot; + str(mean_score) + &quot;, max: &quot; + str(max(self.scores)) + &quot;)\n&quot;)  
    if mean_score &gt;= AVERAGE_SCORE_TO_SOLVE and len(self.scores) &gt;= CONSECUTIVE_RUNS_TO_SOLVE:  
        solve_score = run-CONSECUTIVE_RUNS_TO_SOLVE  
        print (&quot;Solved in &quot; + str(solve_score) + &quot; runs, &quot; + str(run) + &quot; total runs.&quot;)  
        self._save_csv(SOLVED_CSV_PATH, solve_score)  
        #self._save_png(input_path=SOLVED_CSV_PATH,  
        #               output_path=SOLVED_PNG_PATH,  
        #               x_label=&quot;trials&quot;,  
        #               y_label=&quot;steps before solve&quot;,  
        #               average_of_n_last=None,  
        #               show_goal=False,  
        #               show_trend=False,  
        #               show_legend=False)  
        exit()  

def _save_png(self, input_path, output_path, x_label, y_label, average_of_n_last, show_goal, show_trend, show_legend):  
    x = []  
    y = []  
    with open(input_path, &quot;r&quot;) as scores:  
        reader = csv.reader(scores)  
        data = list(reader)  
        for i in range(0, len(data)):  
            x.append(int(i))  
            y.append(int(data[i][0]))  

    plt.subplots()  
    plt.plot(x, y, label=&quot;score per run&quot;)  

    average_range = average_of_n_last if average_of_n_last is not None else len(x)  
    plt.plot(x[-average_range:], [np.mean(y[-average_range:])] * len(y[-average_range:]), linestyle=&quot;--&quot;, label=&quot;last &quot; + str(average_range) + &quot; runs average&quot;)  

    if show_goal:  
        plt.plot(x, [AVERAGE_SCORE_TO_SOLVE] * len(x), linestyle=&quot;:&quot;, label=str(AVERAGE_SCORE_TO_SOLVE) + &quot; score average goal&quot;)  

    if show_trend and len(x) &gt; 1:  
        trend_x = x[1:]  
        z = np.polyfit(np.array(trend_x), np.array(y[1:]), 1)  
        p = np.poly1d(z)  
        plt.plot(trend_x, p(trend_x), linestyle=&quot;-.&quot;,  label=&quot;trend&quot;)  

    plt.title(self.env_name)  
    plt.xlabel(x_label)  
    plt.ylabel(y_label)  

    if show_legend:  
        plt.legend(loc=&quot;upper left&quot;)  

    plt.savefig(output_path, bbox_inches=&quot;tight&quot;)  
    plt.close()  

def _save_csv(self, path, score):  
    if not os.path.exists(path):  
        with open(path, &quot;w&quot;):  
            pass  
    scores_file = open(path, &quot;a&quot;)  
    with scores_file:  
        writer = csv.writer(scores_file)  
        writer.writerow([score])  
</code></pre>

<p>#%%
`</p>
</body>
</html>
