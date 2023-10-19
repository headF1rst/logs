#papercut 

[[JMH]] 결과 리포트를 시각화하여 볼 수 있다.

```bash
plugins {
	id 'me.champeau.jmh' version '0.7.1'
	id 'io.morethan.jmhreport' version '0.9.0'
}

jmh {
	resultFormat = 'JSON'
}

jmhReposrt {
	jmhResultPath = project.file('build/results/jmh/result.json')
	jmhReportOutput = project.file('build/results/jmh')
}
```


