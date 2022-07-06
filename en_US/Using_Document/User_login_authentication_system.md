# DataSphere Studio User login authentication system

> DSS only provides an administrator account by default, and user login authentication relies on Linkis' LDAP user login authentication system.
> This article will introduce the user login authentication methods currently supported by Linkis in detail.

## 1. Basic introduction

1. The user name of the DSS super administrator is the deployment user name. If the deployment user is hadoop, the user name and password of the administrator are hadoop/hadoop. The specific user can be found in [DSS single-machine deployment document](../Installation/Deployment/DSS&Linkis One-click deployment document stand-alone version.md) view.

2. The user login authentication of DSS relies on the user login authentication system of Linkis. In addition to administrators, Linkis also supports the following user login authentication methods:

    - Token login method
    - Proxy user mode
    - Access to SSO single sign-on  

## 2. Token login method

This method is used for third-party systems to access Linkis and DSS.

When the third-party system calls the Linkis and DSS background interfaces, the login can be skipped directly through the token mode.

Specify the following parameters in `linkis/linkis-gateway/conf/linkis.properties`:

```properties
    # Open token mode
    wds.linkis.gateway.conf.enable.token.auth=true
    # Specify the token configuration file
    wds.linkis.gateway.conf.token.auth.config=token.properties
```

In the `linkis/linkis-gateway/conf` directory, create a `token.properties` file with the following content:

```properties
    # The format is as follows：
    ${TOKEN_NAME}=${USER1},${USER2}
    # E.g：
    AZKABAN=*
```

TOKEN_NAME refers to the tokenId assigned to the third-party system, and the following value is the user who can skip login. If all requests from the system are fully trusted, it can be directly equal to *, indicating full authorization.

When a third-party system requests DSS and Linkis, write the following two parameters in the request header or cookie:

```json
    {
      "Token-Code": "${TOKEN_NAME}",
      "Token-User": "${USER}"
    }
```

## 3. Proxy user mode

This method allows the login user to be different from the user who actually uses DSS. The main function is to control that the user must be a real-name user when logging in, but when the big data platform is actually used, it is a non-real-name user.

Specify the following parameters in `linkis/linkis-gateway/conf/linkis.properties`:

```properties
    # Turn on proxy mode
    wds.linkis.gateway.conf.enable.proxy.user=true
    # Specify proxy configuration file
    wds.linkis.gateway.conf.proxy.user.config=proxy.properties
```

In the `linkis/linkis-gateway/conf` directory, create a `proxy.properties` file with the following content:

```properties
    # The format is as follows：
    ${LOGIN_USER}=${PROXY_USER}
    # E.g：
    enjoyyin=hadoop
```

If the existing proxy mode does not meet your needs, you can also modify it manually:`com.webank.wedatasphere.linkis.gateway.security.ProxyUserUtils`。

## 4. Access SSO single sign-on

Accessing your company's SSO single sign-on system is a bit more complicated.

First, you need to enable SSO single point authentication function, please specify the following parameters in `linkis/linkis-gateway/conf/linkis.properties`:

```properties
    wds.linkis.gateway.conf.enable.sso=true
```

Then you need to implement the SSOInterceptor interface:

```scala
trait SSOInterceptor {

  /**
    * If the SSO single sign-on function is enabled, after the front-end jumps to the SSO login page and the login is successful, it will jump back to the DSS home page again. At this time, the DSS front-end requests the gateway again.
    * The gateway will call this method to obtain the user who has logged in with SSO, and then write the user to the cookie to ensure that subsequent requests can be released directly.
    * You need to implement this method to return the username through Request.
    * @param gatewayContext
    * @return
    */
  def getUser(gatewayContext: GatewayContext): String

  /**
    * Through the DSS homepage URL, the user generates a redirectable SSO login page URL.
    * Requirement: You need to bring requestUrl, so that you can jump back after the SSO login is successful
    * @param requestUrl DSS homepage URL
    * @return 例如：https://${sso_host}:${sso_port}/cas/login?redirectUrl=${requestUrl}
    */
  def redirectTo(requestUrl: URI): String

  /**
    * When the user logs out, the gateway will call this interface to ensure that after the gateway clears the cookie, the SSO single sign-on will also clear the login information
    * @param gatewayContext
    */
  def logout(gatewayContext: GatewayContext): Unit

}
```

Type your SSO implementation class into a jar package and put it in the `linkis/linkis-gateway/lib` directory.

Linkis provides two ways to load your SSO implementation class:

Declaring the SSO implementation class as a Spring bean requires you to simply annotate the class name with `@Component`.

Specify the following parameters in `linkis/linkis-gateway/conf/linkis.properties`:

```properties
    #Please specify as your SSO implementation class
    wds.linkis.gateway.conf.sso.interceptor=com.webank.wedatasphere.linkis.gateway.security.sso.SSOInterceptor
```

Restart linkis-gateway, SSO single sign-on will take effect.