# Acceso al laboratorio
https://www.katacoda.com/openshift/courses/playgrounds/openshift42

# Asignando los permisos necesarios
oc adm policy add-cluster-role-to-user cluster-admin admin

# Instalar todos los Operadores

# Instalar el plano de control :coffee::coffee:

# Service Mesh Member Roll

# Clonar el repositorio
git clone https://github.com/erickeliseo/sm.git

# Desplegando el aplicativo de demo
oc apply -n bookinfo -f bookinfo.yaml  
oc apply -n bookinfo -f bookinfo-gateway.yaml  
oc apply -n bookinfo -f destination-rule-all-mtls.yaml  
oc get pods -n bookinfo  

# Generando trafico
export GATEWAY_URL=$(oc -n istio-system get route istio-ingressgateway -o jsonpath='{.spec.host}')  
watch curl -o /dev/null -s -w "%{http_code}\n" http://$GATEWAY_URL/productpage  

# Instalando MongoDB y Ratings-v2 / Canary Release
oc apply -n bookinfo -f vs-dr-ratings.yaml  
oc apply -n bookinfo -f bookinfo-db.yaml  
oc apply -n bookinfo -f bookinfo-ratings-v2.yaml  
oc apply -n bookinfo -f vs-dr-ratings-75-25.yaml  
oc apply -n bookinfo -f vs-dr-ratings-0-100.yaml  

# Reviews 80/20
oc apply -n bookinfo -f virtual-service-reviews-80-20.yaml

# Details Canary
oc apply -n bookinfo -f  virtual-service-details-v1.yaml  
oc apply -n bookinfo -f bookinfo-details-v2.yaml  
oc apply -n bookinfo -f virtual-service-details-80-20.yaml  
oc apply -n bookinfo -f  virtual-service-details-0-100.yaml  

# Jaeger UI
