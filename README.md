# UnityJsonHelper
JsonHelper is a small, simple utility class designed to allow you to grab a json object (or objects) that match the provided key.
The reason for this class, is that you probably want to have json files with several objects worth of data, but JsonUtility doesn't
provide any way of handling that.

Some example use:

Hero.json
```json
{
  "pyroKid": {
    "attack": {
      "damage": 50,
      "reload": 1.8
    },
    "defense": {
      "health": 120,
      "regen": 5
    }
  },
  "iceKid": {
    "attack": {
      "damage": 20,
      "reload": 0.6
    },
    "defense": {
      "health": 170,
      "regen": 8
    }
}
```

SomeUnityCode.cs
```csharp
using UnityEngine;
using System;
using System.IO;

// look up JsonUtility in the Unity api if this bit confuses you
[Serializable]
public class AttackInfo
{
  public float damage;
  public float reload;
}

// . . .

public void SomeGameFunction()
{
  // gets the pyro kid json string
  string pyroKidJson = JsonHelper.GetJsonObject(File.ReadAllText(jsonFilePath), "pyroKid");
  
  // gets pyro kid's attack component
  string pyroAttackJson = JsonHelper.GetJsonObject(pyroKidJson, "attack");
  
  // and then we get our attack component as an object!
  AttackInfo info = JsonUtility.FromJson<AttackInfo>(pryoAttackJson);
  // doing things piece-wise like this is kinda like navigating through the json file
  // I may add a better way of doing this in the future, ex GetJsonObjectFromPath(jsonStr, "pyroKid/attack")
  
  //Alternatively, if we want all the "attack" objects
  //The first argument is the JSON string to read from
  //The second argument is the key to search for
  
  //Create List for storing our results
  List<AttackInfo> attackInfos = new List<AttackInfo>();
  
  //Get array of json string objects
  string[] allAttackJsons = JsonHelper.GetJsonObjects(File.ReadAllText(jsonFilePath), "attack");
  
  //Iterate through results and add each result to our List.
  foreach (string jsonObj in allAttackJsons)
  {
    attackInfos.Add(JsonUtility.FromJson<AttackInfo>(jsonObj));
  }
  // and now we have a list of all attack components in the json file!
  // Not very useful in this hypothetical example,
  // but useful in other situations like loading save data
  
  //This method will simply return all objects
  //Overloaded method which will return all objects
  List<AttackInfo> attackInfos = new List<AttackInfo>();
  
  //gets string array of json string objects
  string[] allJSON = JsonHelper.GetJsonObjects(File.ReadAllText(jsonFilePath));
  
  foreach (string jsonObj in allJSON)
  {
    attackInfos.Add(JsonUtility.FromJson<AttackInfo>(jsonObj));
  }
  //Convenient for iterating through a list of generic JSON objects and compiling structured data.
}
```
