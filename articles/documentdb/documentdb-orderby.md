---
redirect_url: https://azure.microsoft.com/services/documentdb/
ROBOTS: NOINDEX, NOFOLLOW
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 0ca716857733290fad4278e3be5059408bb75393
ms.lasthandoff: 03/29/2017


---
# <a name="sorting-documentdb-data-using-order-by"></a>Ordenación de datos de DocumentDB con Order By
Microsoft Azure DocumentDB admite la consulta de documentos mediante documentos de SQL sobre JSON. Los resultados de la consulta se pueden ordenar mediante la cláusula ORDER BY en instrucciones de consulta SQL.

Después de leer este artículo, podrá responder a las preguntas siguientes: 

* ¿Cómo se consulta con Order By?
* ¿Cómo se configura una directiva de indexación para Order By?
* ¿Qué novedades se esperan?

También se ofrecen [Ejemplos](#samples) y [Preguntas más frecuentes](#faq).

Para obtener una referencia completa sobre las consultas de SQL, consulte el [tutorial de consultas de DocumentDB](documentdb-sql-query.md).

## <a name="how-to-query-with-order-by"></a>Cómo realizar consultas con Order By
Al igual que en ANSI-SQL, ahora puede incluir una cláusula Order By opcional en las instrucciones SQL al consultar DocumentDB. La cláusula puede incluir un argumento ASC o DESC opcional para especificar el orden en que se deben recuperar los resultados. 

### <a name="ordering-using-sql"></a>Ordenación mediante SQL
Por ejemplo, aquí se muestra una consulta para recuperar los diez libros principales en orden descendente por sus títulos. 

    SELECT TOP 10 * 
    FROM Books 
    ORDER BY Books.Title DESC

### <a name="ordering-using-sql-with-filtering"></a>Ordenación mediante SQL con filtrado
Puede ordenar con cualquier propiedad anidada dentro de documentos como Books.ShippingDetails.Weight y puede especificar filtros adicionales en la cláusula WHERE junto con Order By, como en este ejemplo:

    SELECT * 
    FROM Books 
    WHERE Books.SalePrice > 4000
    ORDER BY Books.ShippingDetails.Weight

### <a name="ordering-using-the-linq-provider-for-net"></a>Ordenación mediante el proveedor de LINQ para .NET
Mediante la versión 1.2.0 del SDK de .NET y posteriores, también puede utilizar la cláusula OrderBy() u OrderByDescending() en consultas de LINQ, como en este ejemplo:

    foreach (Book book in client.CreateDocumentQuery<Book>(UriFactory.CreateDocumentCollectionUri("db", "books"))
        .OrderBy(b => b.PublishTimestamp)
        .Take(100))
    {
        // Iterate through books
    }

DocumentDB admite la ordenación con una única propiedad numérica, de cadena o booleana por consulta, con tipos de consulta adicionales que estarán disponibles próximamente. Consulte [Qué novedades se esperan](#Whats_coming_next) para obtener más detalles.

## <a name="configure-an-indexing-policy-for-order-by"></a>Configurar una directiva de indexación para Order By
Recuerde que DocumentDB admite dos tipos de índices (Hash y de intervalo), que se pueden establecer para propiedades/rutas de acceso específicas, tipos de datos (cadenas/números) y en valores de precisión diferentes (precisión máxima o un valor de precisión fijo). Puesto que DocumentDB usa la indexación Hash de manera predeterminada, debe crear una nueva colección con una directiva de indexación personalizada con el intervalo en los números, cadenas o ambos, para poder usar Order By. 

> [!NOTE]
> Los índices de intervalo de cadena se introdujeron en el 7 de julio de 2015 con la API de REST versión 2015-06-03. Para crear directivas de Order By con cadenas, debe usar el SDK versión 1.2.0 del SDK de .NET o la versión 1.1.0 del SDK Python, Node.js o Java.
> 
> Antes de la API de REST versión 2015-06-03, la directiva de indexación de colección predeterminada era Hash para cadenas y números. Esto se cambió a Hash para cadenas e Intervalo para números. 
> 
> 

Para obtener más información, consulte [Directivas de indexación de DocumentDB](documentdb-indexing-policies.md).

### <a name="indexing-for-order-by-against-all-properties"></a>Indexación para Order By en todas las propiedades
A continuación se indica cómo puede crear una colección con la indexación "Todo el intervalo" para Order By con cualquiera/todas las propiedades numéricas o de cadena que aparecen en documentos JSONany/all numérico o cadena de las propiedades que aparecen en los documentos JSON dentro de esta. Aquí se muestra cómo reemplazar el tipo de índice predeterminado para los valores de cadena por Intervalo y con la precisión máxima (-1).

    DocumentCollection books = new DocumentCollection();
    books.Id = "books";
    books.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), books);  

> [!NOTE]
> Tenga en cuenta que Order By solo devolverá resultados de los tipos de datos (cadena y número) que están indexados con un RangeIndex. Por ejemplo, si tiene la directiva de indexación predeterminada que solo tiene RangeIndex en números, una consulta Order By en una ruta de acceso con valores de cadena no devolverá ningún documento.
> 
> 

### <a name="indexing-for-order-by-for-a-single-property"></a>Indexación de Order By para una sola propiedad
A continuación se indica cómo puede crear una colección con indexación para Order By con la propiedad Title, que es una cadena. Hay dos rutas de acceso, una para la propiedad Title ("/Title/?") con la indexación de intervalo y la otra para todas las otras propiedades con el esquema de indexación predeterminado, que es Hash para cadenas e Intervalo para los números.                    

    booksCollection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 } } 
            });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), booksCollection);  


## <a name="samples"></a>Muestras
Eche un vistazo al [proyecto de ejemplos de GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) en el que se muestra cómo se usa Order By (Ordenar por), incluida la creación de directivas de indexación y la paginación con Order By (Ordenar por). Los ejemplos son de código abierto y le animamos a que envíe solicitudes de extracción con las contribuciones que podrían ayudar a otros desarrolladores de DocumentDB. Consulte las [directrices de contribución](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md) para obtener instrucciones acerca de cómo realizar sus aportaciones.  

## <a name="faq"></a>Preguntas más frecuentes
**¿Cuál es el consumo de unidades de solicitud (RU) esperado de las consultas de Order By?**

Dado que Order By usa el índice de DocumentDB para las búsquedas, el número de unidades de solicitud que consumen las consultas de Order By será similar a las consultas equivalentes sin Order By. Al igual que con cualquier otra operación en DocumentDB, el número de unidades de solicitud depende de los tamaños y formas de los documentos, así como la complejidad de la consulta. 

**¿Cuál es la sobrecarga de indexación esperada de Order By?**

La sobrecarga del almacenamiento de indexación será proporcional a la cantidad de propiedades. En el peor escenario posible, la sobrecarga del índice será el 100 % de los datos. No hay diferencias en la sobrecarga de procesamiento (unidades de solicitud) entre la indexación de intervalo u Order By y la indexación de Hash predeterminada.

**¿Cómo se consultan los datos existentes en DocumentDB con Order By?**

Para ordenar los resultados de la consulta con Order By, debe modificar la directiva de indexación de la colección para usar un tipo de índice de intervalo en la propiedad que se usa para ordenar. Consulte [Modificación de la directiva de indexación](documentdb-indexing-policies.md#modifying-the-indexing-policy-of-a-collection). 

**¿Cuáles son las limitaciones actuales de Order By?**

Order By solo se puede especificar para una propiedad, numérica o de cadena, cuando está indexada mediante intervalo con la precisión máxima (-1).

No puede realizar las siguientes operaciones:

* Order By con propiedades de cadena internas com id, _rid y _self (próximamente).
* Order By con propiedades derivadas del resultado de una combinación de dentro de documentos (próximamente).
* Order By con varias propiedades (próximamente).
* Order By con consultas en bases de datos, colecciones, usuarios, permisos o datos adjuntos (próximamente).
* Order By con las propiedades calculadas (por ejemplo, el resultado de una expresión o una función UDF o integrada).

Order By no se admite actualmente para consultas entre particiones al usar el Explorador de consultas en Azure Portal.

## <a name="troubleshooting"></a>Solución de problemas
Si recibe un error que indica que Order By no es compatible, asegúrese de que usa una versión del [SDK](documentdb-sdk-dotnet.md) que sea compatible con Order By. 

## <a name="next-steps"></a>Pasos siguientes
Bifurque el [proyecto de ejemplos de GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) y comience a ordenar sus datos. 

## <a name="references"></a>Referencias
* [Referencia de las consultas de DocumentDB](documentdb-sql-query.md)
* [Referencia de la directiva de indexación de DocumentDB](documentdb-indexing-policies.md)
* [Referencia SQL de DocumentDB](https://msdn.microsoft.com/library/azure/dn782250.aspx)
* [Ejemplos de Order By de DocumentDB](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)


