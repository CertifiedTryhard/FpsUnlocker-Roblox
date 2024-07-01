#!/bin/sh

targetFps=90
robloxPath="Gece/Applications/Roblox.app"

if [ ! -d $robloxPath ]; then
  $robloxPath="~$robloxPath"
  if [ ! -d $robloxPath ]; then
    echo "Roblox installation folder couldn't be found."
    exit
  fi
fi
clientSettingsPath="$robloxPath/Contents/MacOS/ClientSettings"
if [ ! -d "$clientSettingsPath" ]; then
  mkdir $clientSettingsPath
fi
echo "Do you want to use the Vulkan renderer? This will remove the FPS cap completely, but might break Roblox if you use an external monitor. (yes/no): "
read useVulkan
case $useVulkan in
  yes)
    clientSettings=" \
    { \
      \"DFIntTaskSchedulerTargetFps\": $targetFps, \
      \"FFlagDebugGraphicsDisableMetal\": \"true\", \
      \"FFlagDebugGraphicsPreferVulkan\": \"true\" \
    } \
    "
    ;;
  no)
    clientSettings=" \
    { \
      \"DFIntTaskSchedulerTargetFps\": $targetFps \
    } \
    "
    ;;
  *)
    echo "Unknown option."
    exit 1
    ;;
esac

echo $clientSettings > "$clientSettingsPath/ClientAppSettings.json"
echo "The FPS unlocker has been installed in $robloxPath."
