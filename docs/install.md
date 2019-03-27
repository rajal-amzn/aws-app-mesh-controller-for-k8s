## Install

Although the controller can be used independently from the aws-app-mesh-inject webhook, it is recommended that they be used together.  To Install the webhook, execute the following commands:

    export MESH_NAME=<my-mesh-name>
    curl https://raw.githubusercontent.com/aws/aws-app-mesh-inject/master/hack/install.sh | bash

This will launch the webhook into the appmesh-inject namespace. Now add the correct permissions to your worker nodes (or your pod identity solution, like kube2iam):

    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "appmesh:DescribeMesh",
                    "appmesh:DescribeVirtualNode",
                    "appmesh:DescribeVirtualService",
                    "appmesh:DescribeVirtualRouter",
                    "appmesh:DescribeRoute",
                    "appmesh:CreateMesh",
                    "appmesh:CreateVirtualNode",
                    "appmesh:CreateVirtualService",
                    "appmesh:CreateVirtualRouter",
                    "appmesh:CreateRoute",
                    "appmesh:UpdateMesh",
                    "appmesh:UpdateVirtualNode",
                    "appmesh:UpdateVirtualService",
                    "appmesh:UpdateVirtualRouter",
                    "appmesh:UpdateRoute",
                    "appmesh:ListMeshes",
                    "appmesh:ListVirtualNodes",
                    "appmesh:ListVirtualServices",
                    "appmesh:ListVirtualRouters",
                    "appmesh:ListRoutes"
                ],
                "Resource": "*"
            }
        ]
    }

Next, launch the controller:

    curl https://raw.githubusercontent.com/aws/aws-app-mesh-controller-for-k8s/v0.1.0/deploy/v0.1.0/all.yaml | kubectl apply -f -

Make sure its ready:

    kubectl wait $(kubectl get pods -n appmesh-system -o name) \
        --for=condition=Ready --timeout=30s -n appmesh-system

Now try following along with [the example](example.md) to get a feel for how it works!