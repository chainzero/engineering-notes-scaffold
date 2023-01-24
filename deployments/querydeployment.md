# QueryDeployment

1\). Akash Deployment Query gRPC endpoints are defined in the Protobuf file located in proto/akash/deployment/v1beta2/query.proto file. &#x20;

The example service used in this review is named Deployment.  This service queries a single deployment and reveals details of that deployment.

```
  rpc Deployment(QueryDeploymentRequest) returns (QueryDeploymentResponse) {
    option (google.api.http).get = "/akash/deployment/v1beta2/deployments/info";
  }
```

2\).  Service definition in Protobuf file spawn function of the same name (Deployment) in the Keeper package (file = x/deployment/keeper/grpc\_query.go).

```
func (k Querier) Deployment(c context.Context, req *types.QueryDeploymentRequest) (*types.QueryDeploymentResponse, error)
```

3\). Within stated file (grpc\_query.go) and the stated function (Deployment) receives parameter of Type `QueryDeploymentRequest`. This type is spawned by Protobuf with the following definition.  The struct is spawned within file node/x/deployment/types/v1beta2/query.pb.go

```
type QueryDeploymentsRequest struct {
	Filters    DeploymentFilters  `protobuf:"bytes,1,opt,name=filters,proto3" json:"filters"`
	Pagination *query.PageRequest `protobuf:"bytes,2,opt,name=pagination,proto3" json:"pagination,omitempty"`
}
```

4\). Within the QueryDeploymentsRequest struct the Filters field is of type DeploymentFilters which is defined by this struct within the node/x/deployment/types/v1beta2/deployment.pb.go.

As defined in this struct the Deployment with filter query accepts filtering using Owner, Dseq, and State declarations.

```
type DeploymentFilters struct {
	Owner string `protobuf:"bytes,1,opt,name=owner,proto3" json:"owner" yaml:"owner"`
	DSeq  uint64 `protobuf:"varint,2,opt,name=dseq,proto3" json:"dseq" yaml:"dseq"`
	State string `protobuf:"bytes,3,opt,name=state,proto3" json:"state" yaml:"state"`
}
```
