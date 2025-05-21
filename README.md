import Foundation

do {
    var rootObject = [String: Any]()

    // Registry Settings
    var registryObject = [String: Any]()
    var keyObject = [String: Any]()
    var entriesArray = [[String: Any]]()

    entriesArray.append(["Name": "ActiveWindowTracking", "Type": "dword", "Value": "00000000"])
    entriesArray.append(["Name": "Beep", "Type": "string", "Value": "No"])
    entriesArray.append(["Name": "DoubleClickHeight", "Type": "string", "Value": "4"])
    entriesArray.append(["Name": "DoubleClickSpeed", "Type": "string", "Value": "650"])
    entriesArray.append(["Name": "MouseSensitivity", "Type": "string", "Value": "6"])
    entriesArray.append(["Name": "MouseSpeed", "Type": "string", "Value": "0"])

    keyObject["Path"] = "HKEY_CURRENT_USER\\Control Panel\\Mouse"
    keyObject["Entries"] = entriesArray
    registryObject["Key"] = keyObject
    rootObject["Registry"] = registryObject

    // Game Configuration
    var configurationObject = [String: Any]()
    configurationObject["freefireth.app~[sensitivity]"] = ""
    configurationObject["directory"] = "emulated/storage/iPhone/freefireth.app/Data/Raw/ios/gameassetbundles/config"
    configurationObject["mainPath"] = "main.path.2019116797"
    configurationObject["versionInfo"] = "1.102.4"
    rootObject["GameConfiguration"] = configurationObject

    // Files
    let filesArray = ["coding.xml", "config.xml", "file.xml"]
    rootObject["Files"] = filesArray

    // MConfigurations
    let mConfigArray = ["aimheadshot.xml"]
    rootObject["MConfigurations"] = mConfigArray

    // Script
    rootObject["Script"] = [
        "aimlock.enemy~target": "\"HEAD\"",
        "aimlock.duck~target": "\"DUCKSHOT\""
    ]

    // Additional Configurations
    let additionalConfigArray = ["dpi.screen", "aimbot.ff"]
    rootObject["AdditionalConfigurations"] = additionalConfigArray

    // MConfigurations for Aim Headshot
    let aimHeadshotMConfigArray = [
        "aimheadshot.ff",
        "sensitivity.weapon",
        "aimlock.enemy",
        "sensitivity.9000",
        "aimfov.90",
        "aimbot.ff"
    ]
    rootObject["MConfigurationsForAimHeadshot"] = aimHeadshotMConfigArray

    // Feature Activations
    let featureActivationsObject = [
        "configuration.aimbot": "on",
        "configuration.aimfov.90": "on",
        "configuration.aimlock": "on",
        "configuration.aimrednumbers": "on",
        "configuration.dpi9000": "on",
        "configuration.anticheat": "on",
        "System.web.configuration.Scipting": "on",
        "configuration.duckshot": "on"
    ]
    rootObject["FeatureActivations"] = featureActivationsObject

    // System Configurations
    let systemConfigurationsObject = [
        "processing": "on",
        "configuration.file.headshot": "on",
        "file.config": "on",
        "file.script": "on",
        "headshot.nocheat": "on",
        "script.aimlock": "on",
        "script.nobanned": "on",
        "configuration.aimbot": "on",
        "script.easyheadshot": "on",
        "script.red.damage.numbers": "on",
        "configuration.aimfov": "on",
        "configuration.dpiscreen9000": "9000",
        "configuration.60fps": "on",
        "configuration.60hz": "on",
        "dpi.sensivity": "9000",
        "screen.dpi.9000": "9000",
        "sensivity.screen.xml": "sensitivity.iphone.xml",
        "script.duckshot": "on",
        "headshot.duckmode": "on",
        "sensitivity.duck": "8500",
        "aimfov.duck": "100"
    ]
    rootObject["SystemConfigurations"] = systemConfigurationsObject

    // Feature for Optimization
    let featureOptimizationObject: [String: Any] = [
        "name": "OPTIMALISASI",
        "target": "screen",
        "description": """
        Adds entries to your Web.config file which are required by any .NET 3.5 AJAX.NET application.
        """,
        "blocks": [
            "block name=\"OPTIMALISASI\" config sections"
        ]
    ]
    rootObject["FeatureOptimization"] = featureOptimizationObject

    // Type
    rootObject["Type"] = "System.Web.Configuration.ScriptingGameServiceWorkAllMode"

    // Game Configurations Array
    let gameConfigArray: [[String: Any]] = [
        [
            "ConfigSettings": [
                "Setting": [
                    "name": "SeguimientoCabezaConMira",
                    "value": "90"
                ]
            ],
            "GameDirectory": [
                "Path": "emulated/storage/iPhone/freefireth.app/Data/Raw/ios/gameassetbundles/config"
            ],
            "MainPath": [
                "Path": "main.path.2019116797"
            ],
            "VersionInfo": [
                "Version": "1.102.4"
            ]
        ]
    ]
    rootObject["GameConfigurations"] = gameConfigArray

    // Convert to XML
    let xmlData = try PropertyListSerialization.data(fromPropertyList: rootObject, format: .xml, options: 0)
    let xmlString = String(data: xmlData, encoding: .utf8)
    print(xmlString ?? "")

} catch {
    print(error)
}
