# Ingress NGINX Controller (WASM)

- Integrates WASM via <https://github.com/Kong/ngx_wasm_module>

- Wasmtime is built in new Docker build stage `rust` & copied into the existing `build` stage:

  ````
  COPY --from=rust /wasmtime-c-api /wasmtime-c-api
  ````


- NGINX is built with the ngx_wasm_module:

  ````
  --add-module=$BUILD_PATH/ngx_wasm_module-$WASM_VERSION
  ````


## To build

(Tested on Intel Mac 2019 + Docker Desktop)

- build Nginx base image

````
$ cd images/nginx
$ make PLATFORMS=linux/amd64

...

#20 exporting to image
#20 exporting layers done
#20 writing image sha256:47de14af4ecc75248c4eeea729e56334f7df4bf6c5568ad7f57c2823e9ee31b6 done
#20 naming to gcr.io/k8s-staging-ingress-nginx/nginx:v20240206-0c3d52bad done
#20 DONE 0.0s
````

- update the NGINX_BASE file with tag.

- build Controller image

  ````
  $ make image
  ..
  writing image sha256:971eee6d97751f2355ce02009271a13655429e3a4a8f8b4a6fa294efbf72db76
  naming to gcr.io/k8s-staging-ingress-nginx/controller:v1.9.5
  ````

- see <https://github.com/markdingram/k8s-nginx-wasm> for usage
