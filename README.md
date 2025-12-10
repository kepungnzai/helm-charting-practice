To print single value 

{{ printf "Value of myVar: %v" .Values.myVar }}

To print entire object 

{{ toYaml .Values | indent 2 }}

Let's say we are trying to create a template for object that might be nil or might 
contains value so we need to check for parent object and then do further checks. 
This can be troublesome. 
 
To check for object nil, we can use "dig" or "get"

Use dig 

{{- $vals := .Values | toJson | fromJson -}}   <- This is to prevent nil 

{{- $workloadIdentityEnabled := dig "security" "workloadIdentity" "enabled" false $vals -}}
{{- $clientId := dig "security" "workloadIdentity" "clientId" "" $vals -}}


Or you want to use "get" here 

 {{- $security := get .Values "security" | default dict -}}
 {{- $workloadIdentity := get $security "workloadIdentity" | default dict -}}
 {{- $workloadIdentityEnabled := $workloadIdentity.enabled | default false -}}
 {{- $clientId := $workloadIdentity.clientId | default "" -}}
