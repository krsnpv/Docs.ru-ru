---
title: "Управление ключами для защиты данных и время жизни в ASP.NET Core"
author: rick-anderson
description: "Дополнительные сведения об управлении ключами для защиты данных и время жизни в ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 0993c68e37944a3aad863b98f92fe0140cfb746d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>Управление ключами для защиты данных и время жизни в ASP.NET Core

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

## <a name="key-management"></a>Управление ключами

Приложение пытается обнаружить его в рабочей среде и обработки конфигурации ключа сам по себе.

1. Если приложение находится в [приложения Azure](https://azure.microsoft.com/services/app-service/), ключи, сохраняются в *%HOME%\ASP.NET\DataProtection-Keys* папки. Эта папка поддерживаемый сети хранения данных и синхронизации на всех компьютерах размещения приложения.
   * Ключи не защищены при хранении.
   * *DataProtection ключей* папку предоставляет ключей ко всем экземплярам приложения в одного слота развертывания.
   * Слоты отдельное развертывание, например промежуточной и производственной сред, не разглашаем кольцо ключ. При переключении между слотами развертывания, например Смена промежуточного хранения в рабочей среде или с использованием / B тестирования, любое приложение, с помощью защиты данных не сможете расшифровать хранимым данным с помощью ключей в предыдущей области памяти. Это приводит к пользователям, журнал из приложения, использующего стандартной проверки подлинности файла cookie ASP.NET Core, как оно использует защиты данных, чтобы защитить свои файлы cookie. При желании колец ключ слот независимый поставщик внешних ключей, такие как хранилище больших двоичных объектов Azure, хранилище ключей Azure, хранилище SQL, или кэш Redis.

1. Если профиль пользователя, ключи, сохраняются в *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* папки. Если операционная система Windows, они шифруются при хранении с помощью DPAPI.

1. Если приложение размещается в службах IIS, ключи, сохраняются в реестре HKLM в специальный раздел реестра, ACLed только для учетной записи рабочего процесса. Неактивные ключи шифруются с помощью API защиты данных.

1. Если ни одно из этих условий не удовлетворяет, ключи не сохранен вне текущего процесса. Когда процесс завершится, все созданные ключи будут потеряны.

Разработчик получает полный контроль над всегда и можно переопределить, как и где хранятся ключи. Первые три параметра выше должен предоставить хорошо значения по умолчанию для большинства приложений, подобно тому, как ASP.NET  **\<machineKey >** работал для автоматического создания процедуры. Последний, резервный параметр — единственный сценарий, который разработчик должен указать [конфигурации](xref:security/data-protection/configuration/overview) переднего плана, если им нужен сохраняемость ключа, но этот fallback возникает только в тех редких случаях.

При размещении в контейнер Docker ключей должно сохраняться в папке, которая является тома Docker (общего тома или подключенного узла тома сохраняется вне пределов продолжительности контейнера) или во внешнем поставщике, таких как [хранилище ключей Azure](https://azure.microsoft.com/services/key-vault/) или [Redis](https://redis.io/). Внешнего поставщика также полезно в сценариях веб-ферм, если приложения нет доступа к общей сетевой диск (в разделе [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) для получения дополнительной информации).

> [!WARNING]
> Если система защиты данных на определенный репозиторий ключа указывает разработчик переопределяет правила, описанные выше, автоматическое шифрование ключей неактивные отключен. Защиты статических можно включить с помощью [конфигурации](xref:security/data-protection/configuration/overview).

## <a name="key-lifetime"></a>Время жизни ключа

По умолчанию ключи имеют время существования 90 дней. По истечении срока действия ключа приложение автоматически создает новый ключ и задает в качестве активного ключа новый ключ. При условии, что удалено ключи остаются в системе, приложение может расшифровать данные, защищенные с ними. В разделе [управление ключами](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) для получения дополнительной информации.

## <a name="default-algorithms"></a>Алгоритмы по умолчанию

Алгоритм защиты полезных данных по умолчанию, используемый — AES-256-CBC по конфиденциальности и HMACSHA256 подлинность. 512-разрядный главного ключа, заменяется каждые 90 дней, используется для формирования двух вложенных ключей, используемых для этих алгоритмов на основе каждого полезных данных. В разделе [подраздел производного](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) для получения дополнительной информации.

## <a name="see-also"></a>См. также

* [Расширяемость управления ключами](xref:security/data-protection/extensibility/key-management)
