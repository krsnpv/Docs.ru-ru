---
title: "Хэширование пароля"
author: rick-anderson
description: "В этом документе объясняется, как для хэширования паролей с помощью интерфейсов API защиты данных ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: e97d17b5f6de2e0ddcde6d51618675388b43911a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="password-hashing"></a>Хэширование пароля

База данных защиты кода включает пакет *Microsoft.AspNetCore.Cryptography.KeyDerivation* которого содержит функции шифрования ключа с наследованием. Данный пакет представляет собой изолированный компонент и не имеет зависимостей от остальной системой защиты данных. Его можно использовать совершенно независимо друг от друга. Наряду с кодом защиты данных, базовый для удобства существует источник.

Пакет сейчас предоставляет метод `KeyDerivation.Pbkdf2` разрешающее хэширования пароля с помощью [PBKDF2 алгоритм](https://tools.ietf.org/html/rfc2898#section-5.2). Этот API очень похожа на .NET Framework существующие [Rfc2898DeriveBytes тип](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), но имеются три важные различия:

1. `KeyDerivation.Pbkdf2` Метод поддерживает использование нескольких PRFs (в настоящее время `HMACSHA1`, `HMACSHA256`, и `HMACSHA512`), тогда как `Rfc2898DeriveBytes` поддерживает только тип `HMACSHA1`.

2. `KeyDerivation.Pbkdf2` Метод обнаруживает текущей операционной системы и пытается выбирать наиболее оптимизированная реализация процедуру, предоставляя гораздо более высокую производительность в определенных случаях. (В Windows 8 предлагает около 10 x пропускную способность `Rfc2898DeriveBytes`.)

3. `KeyDerivation.Pbkdf2` Метод требует указать все параметры (хэшу, PRF и число итераций). `Rfc2898DeriveBytes` Тип предоставляет значения по умолчанию для этих.

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

См. исходный код для ASP.NET Core удостоверения `PasswordHasher` вариант использования тип для реального мира.
