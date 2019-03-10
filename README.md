# Dragoons Databases

This proyect contains all the databases of Dragoons for our cluster

helm install --namespace jx-staging viserion/drgdb --name data
Repeat the command until it pushes trough

helm install --name userdb stable/postgresql --namespace jx-staging --set postgresqlUsername=drgmsuser,postgresqlDatabase=drgMsUser,pgHbaConfiguration=host drgMsUser drgmsuser 0.0.0.0/0 trust

kubectl port-forward --namespace jx-staging userdb-postgresql-0 5432:5432 
& psql --host 127.0.0.1 -U drgmsuser

kubectl port-forward --namespace jx-staging data-user-postgres-0 5432:5432
