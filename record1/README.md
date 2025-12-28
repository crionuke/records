Record #1

I tried to run docker-otel-lgtm (an all-in-one Docker image for the Grafana observability stack) with custom Grafana
dashboards. Both ChatGPT and Claude suggested mounting configuration files to the wrong paths inside the container. As a
result, the dashboards did not appear.

I found the correct locations by inspecting the container filesystem and comparing example dashboards with their
provisioning configuration, but it took about an hour.

A working compose.yaml is available in the repository:
https://github.com/crionuke/chunks/blob/main/record1

#records #grafana