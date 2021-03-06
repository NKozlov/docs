## Авторизованные ключи {#keys}

_Авторизованные ключи_ — это пара ключей (открытый и закрытый), которые используются при создании [JSON Web Token](https://tools.ietf.org/html/rfc7519). JSON Web Token необходим при запросе IAM-токена для сервисного аккаунта.

Закрытый ключ возвращается при создании новой пары ключей, а открытый ключ содержится в ресурсе [Key](/docs/iam/api-ref/Key/).

>[!IMPORTANT]
>
>Закрытый ключ сервисного аккаунта — это секретная информация, позволяющая выполнять операции в Яндекс.Облаке. Храните закрытый ключ в надежном месте.
