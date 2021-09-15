# GoIsilon

## Overview
GoIsilon is a Go package that provides a client for the EMC Isilon OneFS HTTP
API. The package provides both direct implementations of the API bindings as
well as abstract, helper functionality. In addition, services such as Docker,
Mesos, and [REX-Ray](http://rexray.readthedocs.io/) use the GoIsilon package
to integrate with the NAS storage platform.

## OneFS API Support Matrix
The GoIsilon package is tested with and supports OneFS 8.1+.

## Examples
The tests provide working examples for how to use the package, but here are
a few code snippets to further illustrate the basic ideas:

### Initialize a new client
This example shows how to initialize a new client.

```go
client, err := NewClient(context.Background())
if err != nil {
	panic(err)
}
```

Please note that there is  no attempt to provide a host, credentials, or any
other options. The `NewClient()` function relies on the following environment
variables to configure the GoIsilon client:

#### Environment Variables
Name | Description
---- | -----------
`GOISILON_ENDPOINT`   | the API endpoint, ex. `https://172.17.177.230:8080`
`GOISILON_USERNAME`   | the username
`GOISILON_GROUP`      | the user's group
`GOISILON_PASSWORD`   | the password
`GOISILON_INSECURE`   | whether to skip SSL validation
`GOISILON_VOLUMEPATH` | which base path to use when looking for volume directories
`GOISILON_VOLUMEPATH_PERMISSIONS` | permissions for new volume directory

### Initialize a new client with options
The following example demonstrates how to explicitly specify options when
creating a client:

```go
client, err := NewClientWithArgs(
	context.Background(),
	"https://172.17.177.230:8080",
	true,
	1,
	"userName",
	"groupName",
	"password",
	"/ifs/volumes")
if err != nil {
	panic(err)
}
```

### Create a Volume
This snippet creates a new volume named "testing" at "/ifs/volumes/loremipsum".
The volume path is generated by concatenating the client's volume path and the
name of the volume.

```go
volume, err := c.CreateVolume(context.Background(), "loremipsum")
```

### Export a Volume
Enabling a volume for NFS access is fairly straight-forward.

```go
if err := c.ExportVolume(context.Background(), "loremipsum"); err != nil {
	panic(err)
}
```


### Delete a Volume
When a volume is no longer needed, this is how it may be removed.

```go
if err := c.DeleteVolume(context.Background(), "loremipsum"); err != nil {
	panic(err)
}
```

### More Examples
Several, very detailed examples of the GoIsilon package in use can be found in
the package's `*_test.go` files as well as in the libStorage Isilon
[storage driver](https://github.com/rexray/rexray/blob/master/libstorage/drivers/storage/isilon/storage/isilon_storage.go).

## Contributions
Please contribute!

Licensing
---------
Licensed under the Apache License, Version 2.0 (the “License”); you may not use
this file except in compliance with the License. You may obtain a copy of the
License at <http://www.apache.org/licenses/LICENSE-2.0>

Unless required by applicable law or agreed to in writing, software distributed
under the License is distributed on an “AS IS” BASIS, WITHOUT WARRANTIES OR
CONDITIONS OF ANY KIND, either express or implied. See the License for the
specific language governing permissions and limitations under the License.

## Support
If you have any questions related to this project, please utilize the Dell EMC container forum located [here.]( https://www.dell.com/community/Containers/bd-p/Containers)
The code and documentation are released with no warranties or SLAs and are intended to be supported through a community driven process.