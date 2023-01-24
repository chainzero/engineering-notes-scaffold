# CreateDeployment

<figure><img src="../.gitbook/assets/AkashCodeBase-Deployments - CosmosLogicalFlow.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/AkashCodeBase-Deployments - AkashDeploymentCreateTX.png" alt=""><figcaption></figcaption></figure>

1\). Akash Deployment Create gRPC endpoints are defined in the Protobuf file located in proto/akash/deployment/v1beta2/service.proto file.  The service is named CreateDeployment.

```
rpc CreateDeployment(MsgCreateDeployment) returns (MsgCreateDeploymentResponse)
```

2\).  Service definition in Protobuf file spawn function of the same name (CreateDeployment) in the Handler package (directory = x/deployment/handler).

3\). Message handler (file = handler.go) matches case based on type of `MsgCreateDeployment`

```
		case *types.MsgCreateDeployment:
			res, err := ms.CreateDeployment(sdk.WrapSDKContext(ctx), msg)
			return sdk.WrapServiceResult(ctx, res, err)
```

4\).  The handler routes the message to the CreateDeployment keeper function found in the Keeper package (file = x/deployment/handler/server.go).

```
func (ms msgServer) CreateDeployment(goCtx context.Context, msg *types.MsgCreateDeployment) (*types.MsgCreateDeploymentResponse, error)
```

5\). Necessary fields are extracted from the message and the deployment struct is passed to the Create function in a corresponding keeper to write the deployment to the blockchain.  Keeper function (Create) is location in x/deployment/keeper/keeper.go.

```
func (k Keeper) Create(ctx sdk.Context, deployment types.Deployment, groups []types.Group) error
```

## CreateDeployment Corresponding Client Interface (CLI)

<figure><img src="../.gitbook/assets/AkashCodeBase-Deployments - AkashDeploymentCreateCLI.png" alt=""><figcaption></figcaption></figure>

1\). Akash CLI command is registered via Cobra for deployment creation (file location = x/deployment/client/cli/tx.go).

```
func cmdCreate(key string) *cobra.Command {
	cmd := &cobra.Command{
		Use:   "create [sdl-file]",
		Short: fmt.Sprintf("Create %s", key),
		Args:  cobra.ExactArgs(1)
```
