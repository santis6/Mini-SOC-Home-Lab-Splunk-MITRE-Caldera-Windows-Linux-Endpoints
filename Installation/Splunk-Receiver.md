# 03 - Habilitar recepción de Forwarders en Splunk

> En este apartado vamos a crear un nuevo receptor de logs en el indexer de Splunk.

---

### En Splunk Web → Settings → Forwarding and receiving → Configure receiving → New Receiving Port

#### Puerto: 9997 (default for UF)

<img width="1346" height="403" alt="settings" src="https://github.com/user-attachments/assets/62c82423-5f42-4224-8727-a8c407ee9204" />


---

<img width="1359" height="628" alt="forwarding and receiving" src="https://github.com/user-attachments/assets/035bf3bc-a00a-4932-b661-5cc57b4b5d2d" />

---

<img width="1359" height="537" alt="add new receiving" src="https://github.com/user-attachments/assets/66666fd9-fc42-4eef-87f6-126832295451" />

---

<img width="1359" height="343" alt="dashboard new receiving port" src="https://github.com/user-attachments/assets/b1d4a4ed-e14d-4764-a7fd-49cd4122d308" />

---


### Validación via CLI (opcional):

#### Verificar que splunkd está escuchando en 9997

```
sudo ss -tulpen | grep 9997
```

<img width="1280" height="100" alt="verificación" src="https://github.com/user-attachments/assets/624171d3-258a-4cfb-83c3-1ce00284a921" />

