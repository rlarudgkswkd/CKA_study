# Other Topics

## Json Path
- Large Data sets
  - 100s of Nodes
  - 1000s of PODs, Deployments, ReplicaSets
- Json path query 가공해서 보게해줌
### How to Json PATH in Kubectl?
- k get nodes -o json
- k get pods -o json
- k get pods -o=jsonpath='{ .items[0].spec.containers[0].image }'
- k get nodes -o=jsonpath='{.items[*].metadata.name}'
- k get nodes -o=jsonpath='{.items[*].metadata.name}{.items[*].status.capacity.cpu}  '
- k get nodes -o=jsonpath='{.items[*].metadata.name}{"\n"}{.items[*].status.capacity.cpu}  '

### Loops - Range
'{range .items[*]} 
  {.metadata.name} {"\t"} {.status.capacity.cpu}{"\n"}{end}'

### Json Path for custom columns
- k get nodes -o=custom-columns=<COLUMN NAME>:<JSON PATH>
- k get nodes -o=custom-columns=NODE:.metadata.name, CPU: .status.capacity.cpu
- k get nodes --sort-by= .metadata.name
- k get nodes --sort-by= .status.capacity.cpu
