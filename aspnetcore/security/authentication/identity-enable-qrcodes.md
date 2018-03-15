---
title: Abilitare la generazione di codice a matrice per le app di autenticazione in ASP.NET Core
author: rick-anderson
description: Informazioni su come abilitare la generazione di codice a matrice per le app di autenticazione che utilizzano l'autenticazione a due fattori ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 09/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: dd326bb32565b743d21e196bcb616a716d7994bf
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2018
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="8b1cf-103">Abilitare la generazione di codice a matrice per le app di autenticazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b1cf-103">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="8b1cf-104">Nota: In questo argomento si applica a ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="8b1cf-104">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="8b1cf-105">ASP.NET Core viene fornito con supporto per le applicazioni per l'autenticazione singoli autenticatore.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="8b1cf-106">Due fattori (2FA) di autenticazione le app di autenticazione, utilizzando un basati sul tempo monouso Password algoritmo (TOTP), sono nel settore dell'approccio per 2FA consigliato.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="8b1cf-107">2FA utilizzando TOTP è preferibile a 2FA SMS.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="8b1cf-108">Un'app di autenticazione fornisce un codice di 6 a 8 cifre che gli utenti devono immettere dopo la conferma di nome utente e password.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="8b1cf-109">In genere un'app di autenticazione viene installata uno Smartphone.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="8b1cf-110">I modelli di applicazione web ASP.NET Core supportano gli autenticatori, ma non offrono supporto per la generazione di QRCode.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="8b1cf-111">I generatori QRCode facilitano l'installazione di 2FA.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="8b1cf-112">Questo documento illustra aggiunta [codice QR](https://wikipedia.org/wiki/QR_code) generazione alla pagina di configurazione 2FA.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="8b1cf-113">Aggiunta di codici a matrice alla pagina di configurazione 2FA</span><span class="sxs-lookup"><span data-stu-id="8b1cf-113">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="8b1cf-114">Queste istruzioni si usa *qrcode.js* dal https://davidshimjs.github.io/qrcodejs/ repository.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-114">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="8b1cf-115">Scaricare il [libreria javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) per il `wwwroot\lib` cartella nel progetto.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-115">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="8b1cf-116">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (pagine Razor) o *Views\Manage\EnableAuthenticator.cshtml* (MVC), individuare il `Scripts` sezione alla fine del file:</span><span class="sxs-lookup"><span data-stu-id="8b1cf-116">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="8b1cf-117">Aggiornamento di `Scripts` sezione per aggiungere un riferimento al `qrcodejs` libreria è stato aggiunto e da una chiamata per generare il codice a matrice.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-117">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="8b1cf-118">Dovrebbe apparire come segue:</span><span class="sxs-lookup"><span data-stu-id="8b1cf-118">It should look as follows:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* <span data-ttu-id="8b1cf-119">Eliminare il paragrafo in cui vengono forniti collegamenti a queste istruzioni.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-119">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="8b1cf-120">Eseguire l'app e assicurarsi che è possibile scansionare il codice a matrice e convalidare il codice che dimostra l'autenticatore.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-120">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="8b1cf-121">Modificare il nome del sito nel codice a matrice</span><span class="sxs-lookup"><span data-stu-id="8b1cf-121">Change the site name in the QR Code</span></span>

<span data-ttu-id="8b1cf-122">Il nome del sito nel codice QR viene ricavato dal nome del progetto che scelto durante la creazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-122">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="8b1cf-123">È possibile modificarla mediante la ricerca del `GenerateQrCodeUri(string email, string unformattedKey)` metodo il *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* file (pagine Razor) o *Controllers\ManageController.cs* file (MVC).</span><span class="sxs-lookup"><span data-stu-id="8b1cf-123">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span> 

<span data-ttu-id="8b1cf-124">Il codice predefinito con il modello è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="8b1cf-124">The default code from the template looks as follows:</span></span>

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

<span data-ttu-id="8b1cf-125">Il secondo parametro nella chiamata a `string.Format` è il nome del sito, ricavato il nome della soluzione.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-125">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="8b1cf-126">Può essere modificata in qualsiasi valore, ma deve essere sempre codificati nell'URL.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-126">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="8b1cf-127">Utilizzo di una libreria di codice a matrice diversi</span><span class="sxs-lookup"><span data-stu-id="8b1cf-127">Using a different QR Code library</span></span>

<span data-ttu-id="8b1cf-128">È possibile sostituire la libreria di codice a matrice con la libreria preferita.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-128">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="8b1cf-129">Il codice HTML contiene un `qrCode` elemento in cui è possibile inserire un codice a matrice da qualsiasi meccanismo nella libreria siano forniti.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-129">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="8b1cf-130">L'URL formattato correttamente per il codice a matrice è disponibile nel:</span><span class="sxs-lookup"><span data-stu-id="8b1cf-130">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="8b1cf-131">`AuthenticatorUri` proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-131">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="8b1cf-132">`data-url` proprietà di `qrCodeData` elemento.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-132">`data-url` property in the `qrCodeData` element.</span></span> 

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="8b1cf-133">TOTP client e server sfasamento dell'ora</span><span class="sxs-lookup"><span data-stu-id="8b1cf-133">TOTP client and server time skew</span></span>

<span data-ttu-id="8b1cf-134">Autenticazione TOTP dipende da dispositivo autenticatore sia il server con l'ora esatta.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-134">TOTP authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="8b1cf-135">Token solo una durata di 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-135">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="8b1cf-136">Se l'account di accesso 2FA TOTP non riesce, verificare che l'ora del server è precisa e preferibilmente sincronizzato a un servizio NTP accurato.</span><span class="sxs-lookup"><span data-stu-id="8b1cf-136">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
