---
title: "Expo APK Not Connecting to Metro Bundler? Try These Fixes"
datePublished: Tue Jul 30 2024 23:00:54 GMT+0000 (Coordinated Universal Time)
cuid: clz90uoaa000108ldg4fq6pzr
slug: expo-apk-not-connecting-to-metro-bundler-try-these-fixes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1721384995637/8c7db362-d56b-4223-afc3-e1544bc14124.png
tags: react-native, expo

---

So, your Expo APK is not connecting to the Metro Bundler? No problem, we'll sort it out. Here are some steps you can follow to fix this issue.

### Solution Steps

1. **Make Sure PC and Phone Are on the Same Network**
    
    The first thing you need to do is ensure that your PC (where the Metro Bundler is running) and your mobile device (where you are running the Expo APK) are on the same network. If they are not on the same network, they won’t be able to communicate.
    
    * **Check Network on Your PC:**
        
        * Go to your PC’s network settings and confirm the network name.
            
    * **Check Network on Your Mobile Device:**
        
        * Go to the Wi-Fi settings on your mobile device and make sure it’s connected to the same network as your PC.
            
2. **Adjust Firewall Settings**
    
    Sometimes, your firewall settings might block the connection between your PC and mobile device. You need to make sure that your firewall is not preventing this connection.
    
    * **Change Firewall Settings on Windows:**
        
        1. Open the Control Panel.
            
        2. Go to **System and Security**.
            
        3. Click on **Windows Defender Firewall**.
            
        4. On the left sidebar, click on **Turn Windows Defender Firewall on or off**.
            
        5. Under the **Private network settings**, select **Turn off Windows Defender Firewall (not recommended)**.
            
        6. Click **OK** to save changes.
            
        
        Note: Turning off your firewall can expose your PC to security risks. Make sure to turn it back on after troubleshooting, or configure it to allow only necessary connections.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1721384800261/aaa0ec69-3f50-4063-9515-de702f47f1c8.png align="center")
        
3. **Check Expo CLI and Metro Bundler Configuration**
    
    Ensure your Expo CLI and Metro Bundler are properly configured and running without issues.
    
    * **Start Metro Bundler:**
        
        * Open your project directory in the terminal.
            
        * Run `npx expo start` to start the Metro Bundler.
            
    * **Verify Metro Bundler URL:**
        
        * Make sure the Metro Bundler is running and note the URL (usually something like `http://localhost:19002`).
            
4. **Restart the Development Server and Device**
    
    Sometimes, simply restarting can resolve connectivity issues.
    
    * **Restart Metro Bundler:**
        
        * Stop the current Metro Bundler process (`Ctrl+C` in terminal).
            
        * Run `npx expo start` again.
            
    * **Restart Your Mobile Device:**
        
        * Power off your mobile device and turn it back on.
            
5. **Use Tunnel Connection**
    
    If you still face issues, try using Expo’s tunnel connection. This method routes the connection through Expo’s servers and can sometimes bypass local network restrictions.
    
    * Run `expo start --tunnel` in your project directory.
        
    * Scan the QR code with your Expo app.
        
6. **Check for IP Address Conflicts**
    
    Ensure there are no IP address conflicts on your network. Sometimes, multiple devices with the same IP address can cause connectivity issues.
    
    * Assign static IP addresses to your PC and mobile device to avoid conflicts.
        

### Wrapping Up

Ensuring that both devices are on the same network, configuring your firewall correctly, and using Expo’s tunnel connection could resolve your issues. Don't forget to revert any changes to your firewall settings while troubleshooting to maintain your PC’s security.

### Additional Resource

* [Github issue](https://github.com/expo/expo-cli/issues/134)