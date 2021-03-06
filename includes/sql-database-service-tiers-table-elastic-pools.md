<!--
Used in:
sql-database-elastic-pool.md   
sql-database-resource-limits.md
sql-database-service-tiers.md  
-->
 
### <a name="basic-elastic-pool-limits"></a>Límites de grupo elástico básico

| Tamaño del grupo (eDTU)  | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Almacenamiento máximo de datos por grupo* | 5 GB | 10 GB | 20 GB | 29 GB | 39 GB | 78 GB | 117 GB | 156 GB |
| Almacenamiento máximo de OLTP en memoria por grupo* | N/D | N/D | N/D | N/D | N/D | N/D | N/D | N/D |
| Máximo número de bases de datos por grupo | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Cantidad máxima de trabajos simultáneos por grupo | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Cantidad máxima de inicios de sesión simultáneos por grupo | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Cantidad máxima de sesiones simultáneas por grupo | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| Cantidad mínima de eDTU por base de datos | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} | {0, 5} |
| Cantidad máxima de eDTU por base de datos | {5} | {5} | {5} | {5} | {5} | {5} | {5} | {5} | {5} |
||||||||

### <a name="standard-elastic-pool-limits"></a>Límites de grupo elástico estándar

| Tamaño del grupo (eDTU)  | **50** | **100** | **200** | **300** | **400** | **800** | 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Almacenamiento máximo de datos por grupo* | 50 GB| 100 GB*| 200 GB | <&300; GB| 400 GB | 800 GB | 
| Almacenamiento máximo de OLTP en memoria por grupo* | N/D | N/D | N/D | N/D | N/D | N/D | 
| Máximo número de bases de datos por grupo | 100 | 200 | 500 | 500 | 500 | 500 | 
| Cantidad máxima de trabajos simultáneos por grupo | 100 | 200 | 400 | 600 |  800 | 1600 |
| Cantidad máxima de inicios de sesión simultáneos por grupo | 100 | 200 | 400 | 600 |  800 | 1600 |
| Cantidad máxima de sesiones simultáneas por grupo | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| Cantidad mínima de eDTU por base de datos | {0,10,20,<br>50} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} |
| Cantidad máxima de eDTU por base de datos | {10,20,<br>50} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | 
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Límites de grupo elástico estándar (continuación) 

| Tamaño del grupo (eDTU)  |  **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Almacenamiento máximo de datos por grupo* | 1,2 TB | 1,6 TB | 2 TB | 2,4 TB | 2,9 TB | 
| Almacenamiento máximo de OLTP en memoria por grupo* | N/D | N/D | N/D | N/D | N/D | 
| Máximo número de bases de datos por grupo | 500 | 500 | 500 | 500 | 500 | 500 |
| Cantidad máxima de trabajos simultáneos por grupo |  2400 | 3200 | 4000 | 5000 | 6000 |
| Cantidad máxima de inicios de sesión simultáneos por grupo |  2400 | 3200 | 4000 | 5000 | 6000 |
| Cantidad máxima de sesiones simultáneas por grupo | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Cantidad mínima de eDTU por base de datos | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} | {0,10,20,<br>50,100} |
| Cantidad máxima de eDTU por base de datos | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | {10,20,<br>50,100} | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Límites de grupo elástico premium

| Tamaño del grupo (eDTU)  | **125** | **250** | **500** | **1000** | **1500** | 
|:---|---:|---:|---:| ---: | ---: | 
| Almacenamiento máximo de datos por grupo* | 250 GB| 500 GB| 750 GB| 750 GB| 750 GB| 
| Almacenamiento máximo de OLTP en memoria por grupo* | 1 GB| 2 GB| 4 GB| 10 GB| 12 GB| 
| Máximo número de bases de datos por grupo | 50 | 100 | 100 | 100 | 100 |  
| Cantidad máxima de trabajos simultáneos por grupo | 200 | 400 | 800 | 1600 |  2400 | 
| Cantidad máxima de inicios de sesión simultáneos por grupo | 200 | 400 | 800 | 1600 |  2400 |
| Cantidad máxima de sesiones simultáneas por grupo | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Cantidad mínima de eDTU por base de datos | {0,25,50,75,<br>125} | {0,25,50,75,<br>125,250} | {0,25,50,75,<br>125,250,500} | {0,25,50,75,<br>125,250,500,<br>1000} | {0,25,50,75,<br>125,250,500,<br>1000,1500} | 
| Cantidad máxima de eDTU por base de datos | {25,50,75,<br>125} | {25,50,75,<br>125,250} | {25,50,75,<br>125,250,500} | {25,50,75,<br>125,250,500,<br>1000} | {25,50,75,<br>125,250,500,<br>1000,1500} |  
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Límites de grupo elástico premium (continuación) 

| Tamaño del grupo (eDTU)  |  **2000** | **2500** | **3000** | **3500** | **4000** |
|:---|---:|---:|---:| ---: | ---: | 
| Almacenamiento máximo de datos por grupo* | 750 GB | 750 GB | 750 GB | 750 GB | 750 GB |
| Almacenamiento máximo de OLTP en memoria por grupo* | 16 GB | 20 GB | 24 GB | 28 GB | 32 GB |
| Máximo número de bases de datos por grupo | 100 | 100 | 100 | 100 | 100 | 
| Cantidad máxima de trabajos simultáneos por grupo |  3200 | 4000 | 4800 | 5600 | 6400 |
| Cantidad máxima de inicios de sesión simultáneos por grupo |  3200 | 4000 | 4800 | 5600 | 6400 |
| Cantidad máxima de sesiones simultáneas por grupo | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Cantidad mínima de eDTU por base de datos | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} | {0,25,50,75,<br>125,250,500,<br>1000,1750} |  {0,25,50,75,<br>125,250,500,<br>1000,1750,4000} | 
| Cantidad máxima de eDTU por base de datos | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750} | {25,50,75,<br>125,250,500,<br>1000,1750,4000} | 
||||||||

### <a name="premium-rs-elastic-pool-limits"></a>Límites de grupo elástico RS Premium

| Tamaño del grupo (eDTU)  | **125** | **250** | **500** | **1000** |
|:---|---:|---:|---:| ---: | ---: | 
| Almacenamiento máximo de datos por grupo* | 250 GB| 500 GB | 750 GB | 750 GB |
| Almacenamiento máximo de OLTP en memoria por grupo* | 1 GB | 2 GB | 4 GB | 10 GB |
| Máximo número de bases de datos por grupo | 50 | 100 | 100 | 100 |
| Cantidad máxima de trabajos simultáneos por grupo | 200 | 400 | 800 | 1600 |
| Cantidad máxima de inicios de sesión simultáneos por grupo | 200 | 400 | 800 | 1600 |
| Cantidad máxima de sesiones simultáneas por grupo | 30000 | 30000 | 30000 | 30000 |
| Cantidad mínima de eDTU por base de datos | {0,25,50,75,<br>125} | {0,25,50,75,<br>125,250} | {0,25,50,75,<br>125,250,500} | {0,25,50,75,<br>125,250,500,<br>1000} |
| Cantidad máxima de eDTU por base de datos | {25,50,75,<br>125} | {25,50,75,<br>125,250} | {25,50,75,<br>125,250,500} | {25,50,75,<br>125,250,500,<br>1000} | 
||||||||

> [!IMPORTANT]
>\* Las bases de datos agrupadas comparten almacenamiento de grupo, por lo que el almacenamiento de los datos en un grupo elástico se limita al almacenamiento de grupo restante o al almacenamiento máximo por base de datos, el que sea menor.
>
