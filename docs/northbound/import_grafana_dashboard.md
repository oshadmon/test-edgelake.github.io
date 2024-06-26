# Importing EdgeLake related dashboards into Grafana

Instructions to create and manage your Grafana instance with EdgeLake, can be found in [Using Grafana](using%20grafana.md) 


The following document provides 3 sample Grafana dashboards
* [Network Map](jsons/network_summary.json) - The dashboard consists of a map showing all the nodes 
in the network, a list of operator nodes and a list of  tables supported in the network.

![grafana_network_map.png](..%2Fimgs%2Fgrafana_network_map.png)

  
* [Random Data Diagram](jsons/rand_data_dashboard.json) - The dashboard consists of a line graph demonstrating min/avg/max, as well gages showing 
the overall number of rows as well as the number of rows per node. The content for these widgets is via our third-party
MQTT client sample connection.  

![grafana_edgex_dashboard.png](..%2Fimgs%2Fgrafana_edgex_dashboard.png)

## Setting Up Grafana

* An [installation of Grafana](https://grafana.com/docs/grafana/latest/setup-grafana/installation/) - We support _Grafana_ version 7.5 and higher, we recommend using _Grafana_ version 9.5.16 or higher. 
<pre>
   <code>
docker run --name=grafana \
  -e GRAFANA_ADMIN_USER=admin \
  -e GRAFANA_ADMIN_PASSWORD=admin \
  -e GF_AUTH_DISABLE_LOGIN_FORM=false \
  -e GF_AUTH_ANONYMOUS_ENABLED=true \
  -e GF_SECURITY_ALLOW_EMBEDDING=true \
  -e GF_INSTALL_PLUGINS=simpod-json-datasource,grafana-worldmap-panel \
  -e GF_SERVER_HTTP_PORT=3000 \
  -v grafana-data:/var/lib/grafana \
  -v grafana-log:/var/log/grafana \
  -v grafana-config:/etc/grafana \
  -it -d -p 3000:3000 --rm grafana/grafana:9.5.16
   </code>
</pre>

Log into Grafana and Declare a _(JSON) Data Source_

1. [Login to Grafana](https://grafana.com/docs/grafana/latest/getting-started/getting-started/) - The default Grafana HTTP port is 3000  
   * URL: http://localhost:3000/ 
   * username: admin | password: admin

<img src="../../imgs/grafana_login.png" alt="Grafana page" width="50%" height="50%" />

2. In _Data Sources_ section, create a new JSON data source
   * select a JSON data source.
   * On the name tab provide a unique name to the connection.
   * On the URL tab add the REST address offered by the EdgeLake node (i.e. http://10.0.0.25:2049)
   * On the ***Custom HTTP Headers***, name the default database. If no header is set, then all EdgeLake hosted databases will be available to a query process.

<table>
  <tr>
    <td><img src="../../imgs/grafana_datasource_connector.png" alt="Data Source Option" /></td>
    <td><img src="../../imgs/grafana_datasource_configuration.png" alt="Data Source Config" /></td>
  </tr>
</table>



## Uploading Dashboard

1. In a new Dashboard goto the _Settings_  
<img src="../../imgs/grafana_base_dashboard.png" alt="Empty Dashboard" />


2. Go _JSON Model_ and add desired model - A model is the JSON object being used to generate the grafana dashboard (for example: [EdgeX Dashboard](../docs/examples/grafana_json/edgex_dashboard.json)).

<table>
  <tr>
    <td><img src="../../imgs/grafana_json_model_empty.png" alt="Empty JSON Model" width="75%" height="75%" /></td>
    <td><img src="../../imgs/grafana_json_model.png" alt="JSON Model" width="75%" height="75%" /></td>
  </tr>
</table>

3. Save Changes


4. Once the changes are saved, you should see a new Dashboard 

<table>
  <tr>
    <td align="center">Before</td>
    <td align="center">After</td>
  </tr>
  <tr>
    <td><img src="../../imgs/grafana_no_dashboard.png" alt="No Dashboards" /></td>
    <td><img src="../../imgs/grafana_new_dashboard.png" alt="New Dashboard" /></td>
  </tr>
</table>

5. For each of the widgets update the following information:
   * Data Source 
   * Metric value (EdgeLake table name)

Once these changes are saved, the outcome should look something like this:

<table>
  <tr>
    <td align="center">View when accessing Dashboard</td>
    <td align="center">Update Data Source</td>
    <td align="center">Update Metric Value</td>
    <td align="center">Outcome</td>
  </tr>
  <tr>
    <td><img src="../../imgs/grafana_edit_button.png" alt="Edit Widget" /></td>
    <td><img src="../../imgs/grafana_update_datasource.png" alt="Update Data Source" /></td>
    <td><img src="../../imgs/grafana_update_table.png" alt="Update Metric Value" /></td>
    <td><img src="../../imgs/grafana_outcome.png" alt="Outcome" /></td>
  </tr>
</table>
