---
title: "Creación de aplicaciones que usan temas y suscripciones de Service Bus | Microsoft Docs"
description: "Introducción a las funcionalidades de publicación-suscripción que ofrecen los temas y suscripciones de Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a48fc9b0-b7b0-464e-8187-a517bf8b4eb4
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/10/2017
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 994a379129bffd7457912bc349f240a970aed253
ms.openlocfilehash: 799ef33c924a0067bb5e8da9d1b4e50091dbabf6


---
# <a name="create-applications-that-use-service-bus-topics-and-subscriptions"></a>Creación de aplicaciones que usan temas y suscripciones de Service Bus
El Bus de servicio de Azure admite un conjunto de tecnologías middleware orientadas a mensajes basadas en la nube, entre las que se incluyen una cola de mensajes de confianza y una mensajería de publicación/suscripción duradera. Este artículo se basa en la información que se proporciona en [Creación de aplicaciones que usan colas de Service Bus](service-bus-create-queues.md) y ofrece una introducción a las funcionalidades de publicación o suscripción que ofrecen los temas de Service Bus.

## <a name="evolving-retail-scenario"></a>Escenario minorista en evolución
Este artículo se prolonga el escenario minorista que se usó en [Creación de aplicaciones que usan colas de Service Bus](service-bus-create-queues.md). Recuerde que los datos de ventas de los terminales de los puntos de venta (PDV) individuales deben enrutarse a un sistema de administración del inventario que usa esos datos para determinar cuándo hay que reponer las existencias. Cada terminal de PDV informa de sus datos de venta mediante el envío de mensajes a la cola **DataCollectionQueue**, donde permanecen hasta que los recibe el sistema de administración del inventario, como se muestra aquí:

![Service Bus 1](./media/service-bus-create-topics-subscriptions/IC657161.gif)

Para desarrollar este escenario, se ha agregado un nuevo requisito al sistema: el propietario del almacén desea poder supervisar el rendimiento de la misma en tiempo real.

Para cumplir este requisito, el sistema debe "iniciar" el flujo de datos de ventas. Deseamos que los mensajes enviados por los terminales de los PDV se envíen al sistema de administración del inventario como antes, pero queremos otra copia de cada mensaje que podamos usar para presentar la vista de panel al propietario del almacén.

En una situación como ésta, en la que se requiere que todos los mensajes los consuman varias partes, puede usar los *temas* de Service Bus. Los temas proporcionan un patrón de publicación o suscripción en el que cada mensaje publicado está disponible para una o varias suscripciones registradas en el tema. En cambio, con las colas cada mensaje lo recibe un único consumidor.

Los mensajes se envían a los temas de la misma manera que a las colas. Sin embargo, los mensajes no se reciben directamente del tema, sino que se reciben de las suscripciones. Una suscripción a un tema se puede considerar una cola virtual que recibe copias de los mensajes que se envían al tema. Los mensajes se reciben de las suscripciones de la misma forma que de las colas.

De vuelta al escenario minorista, la cola se reemplaza por un tema y se agrega una suscripción, que el componente del sistema de administración del inventario puede usar. El sistema aparece ahora como sigue:

![Service Bus 2](./media/service-bus-create-topics-subscriptions/IC657165.gif)

Aquí la configuración se comporta de forma idéntica al diseño anterior basado en cola. Es decir, los mensajes enviados al tema se enrutan a la suscripción **Inventory**, desde donde los consume el **sistema de administración del inventario**.

Para admitir el panel de administración, se crea una segunda suscripción en el tema, como se muestra aquí:

![Service Bus 3](./media/service-bus-create-topics-subscriptions/IC657166.gif)

Con esta configuración, cada mensaje de los terminales de PDV queda a disposición de las suscripciones **Dashboard** e **Inventory**.

## <a name="show-me-the-code"></a>Visualización del código
En el artículo [Creación de aplicaciones que usan colas de Service Bus](service-bus-create-queues.md) se describe cómo registrarse en una cuenta de Azure y crear un espacio de nombres del servicio. Para usar un espacio de nombres de Bus de servicio, una aplicación debe hacer referencia el ensamblado del Bus de servicio, en concreto Microsoft.ServiceBus.dll. La manera más fácil de hacer referencia a las dependencias de Service Bus es instalar el [paquete Nuget](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) de Service Bus. El ensamblado también se puede encontrar como parte del SDK de Azure. La descarga está disponible en la [página de descarga del SDK de Azure](https://azure.microsoft.com/downloads/).

### <a name="create-the-topic-and-subscriptions"></a>Creación del tema y de las suscripciones
Las operaciones de administración de las entidades de mensajería de Service Bus (colas y temas de publicación o suscripción) se realizan a través de la clase [NamespaceManager](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager). Se requieren credenciales apropiadas para crear una instancia de **NamespaceManager** para un espacio de nombres concreto. Service Bus usa un modelo de seguridad basado en la [firma de acceso compartido (SAS)](service-bus-sas-overview.md). La clase [TokenProvider](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.tokenprovider#microsoft_servicebus_tokenprovider) representa un proveedor de tokens de seguridad con métodos de generador integrados que devuelven algunos proveedores de tokens conocidos. Vamos a usar un método [CreateSharedAccessSignatureTokenProvider](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) para retener las credenciales de SAS. A continuación, se construye la instancia de [NamespaceManager](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) con la dirección base del espacio de nombres de Service Bus y el proveedor de tokens.

La clase [NamespaceManager](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager) proporciona métodos para crear, enumerar y eliminar entidades de mensajes. El siguiente código muestra cómo se crea la instancia **NamespaceManager** y se usa para crear el tema **DataCollectionTopic**.

```csharp
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", "test-blog", string.Empty);
string name = "RootManageSharedAccessKey";
string key = "abcdefghijklmopqrstuvwxyz";

TokenProvider tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(name, key);
NamespaceManager namespaceManager = new NamespaceManager(uri, tokenProvider);

namespaceManager.CreateTopic("DataCollectionTopic");
```

Tenga en cuenta que hay sobrecargas del método[CreateTopic](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager#Microsoft_ServiceBus_NamespaceManager_CreateTopic_System_String_) que permiten establecer las propiedades del tema. Por ejemplo, puede establecer el valor predeterminado del período de vida (TTL) de los mensajes enviados al tema. A continuación, agregue las suscripciones **Inventory** y **Dashboard**.

```csharp
namespaceManager.CreateSubscription("DataCollectionTopic", "Inventory");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard");
```

### <a name="send-messages-to-the-topic"></a>Envío de mensajes al tema
En el caso de operaciones en tiempo de ejecución en entidades de Service Bus (por ejemplo, enviar y recibir mensajes), una aplicación debe crear primero un objeto [MessagingFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.messagingfactory#microsoft_servicebus_messaging_messagingfactory). De forma parecida a la clase [NamespaceManager](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.namespacemanager#microsoft_servicebus_namespacemanager), la instancia de **MessagingFactory** se crea a partir de la dirección base del espacio de nombres del servicio y del proveedor de tokens.

```
MessagingFactory factory = MessagingFactory.Create(uri, tokenProvider);
```

Los mensajes enviados a los temas de Service Bus, y los recibidos de ellos, son instancias de la clase [BrokeredMessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage). Esta clase consta de un conjunto de propiedades estándar (como [Label](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) y [TimeToLive](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), un diccionario que se usa para conservar las propiedades de la aplicación y un cuerpo de datos de aplicación arbitrarios. Una aplicación puede establecer el cuerpo pasando cualquier objeto serializable (el siguiente ejemplo pasa un objeto **SalesData** que los datos de ventas del terminal del PDV), que usará [DataContractSerializer](https://msdn.microsoft.com/library/system.runtime.serialization.datacontractserializer.aspx) para serializar el objeto. También se puede proporcionar un objeto [Stream](https://msdn.microsoft.com/library/system.io.stream.aspx).

```csharp
BrokeredMessage bm = new BrokeredMessage(salesData);
bm.Label = "SalesReport";
bm.Properties["StoreName"] = "Redmond";
bm.Properties["MachineID"] = "POS_1";
```

La manera más fácil de enviar mensajes al tema es usar [CreateMessageSender](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageSender_System_String_) para crear un objeto [MessageSender](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.messagesender) directamente desde la instancia de [MessagingFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.messagingfactory).

```csharp
MessageSender sender = factory.CreateMessageSender("DataCollectionTopic");
sender.Send(bm);
```

### <a name="receive-messages-from-a-subscription"></a>Recepción de mensajes de una suscripción
Al igual que cuando se usan las colas, para recibir mensajes desde una suscripción se puede usar un objeto [MessageReceiver](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.messagereceiver) que se crea directamente desde [MessagingFactory](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.messagingfactory) con [CreateMessageReceiver](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_CreateMessageReceiver_System_String_). Puede usar uno de los dos modos de recepción (**ReceiveAndDelete** y **PeekLock**), como se describe en [Creación de aplicaciones que usan colas de Service Bus](service-bus-create-queues.md).

Tenga en cuenta que cuando se crea un objeto **MessageReceiver** para las suscripciones, el parámetro *entityPath* tiene la forma `topicPath/subscriptions/subscriptionName`. Por lo tanto, para crear un objeto **MessageReceiver** para la suscripción **Inventory** del tema **DataCollectionTopic**, *entityPath* debe establecerse en `DataCollectionTopic/subscriptions/Inventory`. El código aparece como sigue:

```csharp
MessageReceiver receiver = factory.CreateMessageReceiver("DataCollectionTopic/subscriptions/Inventory");
BrokeredMessage receivedMessage = receiver.Receive();
try
{
    ProcessMessage(receivedMessage);
    receivedMessage.Complete();
}
catch (Exception e)
{
    receivedMessage.Abandon();
}
```

## <a name="subscription-filters"></a>Filtros de suscripción
Hasta ahora, en este escenario todos los mensajes enviados al tema han estado disponibles para todas las suscripciones registradas. Aquí las palabras clave son "han estado a disposición". Mientras que las suscripciones de Service Bus ven todos los mensajes enviados al tema, solo se puede copiar un subconjunto de dichos mensajes a la cola de suscripción virtual. Esto se realiza mediante los *filtros* de suscripción. Al crear una suscripción, se puede especificar una expresión de filtro que tenga la forma de un predicado de estilo SQL92 que opere a través de las propiedades del mensaje, tanto las propiedades de sistema (por ejemplo, [Label](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label)) como las propiedades de la aplicación, como **StoreName** en el ejemplo anterior.

Al desarrollar el escenario para ilustrarlo, es preciso agregar un segunda almacén al escenario minorista. Los datos de ventas de todos los terminales de los PDV de ambos almacenes deben enrutarse al sistema de gestión centralizada de inventarios, pero un administrador de almacén que use la herramienta Panel solo está interesado en el rendimiento de dicho almacén. Para hacerlo, se puede usar el filtrado de suscripciones. Tenga en cuenta que cuando los terminales de los PDV publican mensajes, establecen la propiedad de la aplicación **StoreName** en el mensaje. Dados dos almacenes, por ejemplo **Redmond** y **Seattle**, los terminales de los PDV de Redmond marcan sus mensajes de datos de ventas con **StoreName** igual a **Redmond**, mientras que los de Seattle usa un **StoreName** igual a **Seattle**. El administrador del almacén de Redmond solo quiere ver los datos de sus terminales de PDV. El sistema aparece como sigue:

![Service-Bus4](./media/service-bus-create-topics-subscriptions/IC657167.gif)

Para configurar este enrutamiento, se crea la suscripción **Dahsboard** como se indica a continuación:

```csharp
SqlFilter dashboardFilter = new SqlFilter("StoreName = 'Redmond'");
namespaceManager.CreateSubscription("DataCollectionTopic", "Dashboard", dashboardFilter);
```

Con este [filtro de suscripción](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sqlfilter), solo se copiarán en la cola virtual de la suscripción **Dashboard** los mensajes que tengan la propiedad **StoreName** establecida en **Redmond**. Sin embargo, hay mucha más información sobre al filtrado de suscripciones. Las aplicaciones pueden tener varias reglas de filtro por suscripción, además de la capacidad de modificar las propiedades de un mensaje cuando pasa a la cola virtual de una suscripción.

## <a name="summary"></a>Resumen
Todas las razones para usar las colas que se describen [Creación de aplicaciones que usan colas de Service Bus](service-bus-create-queues.md) también se aplican a los temas, en concreto:

* Desacoplamiento temporal: no es preciso que los productores y consumidores de mensajes estén en línea al mismo tiempo.
* Nivelación de carga: los picos de carga se suavizan con el tema, lo que permite aprovisionar las aplicaciones que consumen para que la carga sea media, en lugar de máxima.
* Equilibrio de carga: es similar a una cola, es posible tener varios consumidores en competencia escuchando una sola suscripción y cada mensaje se entrega solo a uno de ellos, con lo que se equilibra la carga.
* Acoplamiento débil: la red de mensajes puede desarrollarse sin que ello afecte a los puntos de conexión existentes; por ejemplo, agregar suscripciones o cambiar filtros a un tema para permitir que haya nuevos consumidores.

## <a name="next-steps"></a>Pasos siguientes
Para más información cómo usar las colas en el escenario minorista de PDV, consulte [Creación de aplicaciones que usan colas de Service Bus](service-bus-create-queues.md).




<!--HONumber=Jan17_HO2-->


