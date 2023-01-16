## Serve K3s config


```js
import { fileURLToPath } from 'url';
import { dirname } from 'path';

const __filename = fileURLToPath(import.meta.url);
const __dirname = dirname(__filename);

import express from 'express';
const app = express();
const port = 8080

app.get('/kubeconfig/:clusterId', (req,res) => {
  console.log(`config for ${req.params.clusterId}: config-k3s${req.params.clusterId}.yml`);
  console.log(`${__dirname}/kubeconfig/config-k3s${req.params.clusterId}.yml`);

  res.sendFile(`${__dirname}/kubeconfig/config-k3s${req.params.clusterId}.yml`);
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
}) 
```