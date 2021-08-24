{{- if .Site.Data.metrics }}
{{- $disruptedServices := slice }}
{{- range $key, $uptimes := .Site.Data.metrics.uptime }}
    {{- if (ne $uptimes.state "ok") }}
        {{- $disruptedServices = $disruptedServices | append $uptimes.name }}
    {{- end }}
{{- end }}
{{- if $disruptedServices }}
---
title: Monitoring Disruption Detected
date: {{ now.UTC.Format "2006-01-02T15:04:05" }}
resolved: false
severity: disrupted
affected:
{{- range $key, $value := $disruptedServices }}
- {{ $value }}
{{- end }}
section: issue
---

The automatic uptime monitoring detected a disruption of service.

{{- end }}
{{- end }}
