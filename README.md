# ISLAND
## IMPORTANT

Since the project file was too large to upload to GitHub, please visit the Google drive link:https://drive.google.com/drive/folders/1jOGc4t22C0EEfBVvQTgmi63AQ2Ziq-kU

As well I have uploaded our demo video to YouTube:https://www.youtube.com/watch?v=tUG7uL7mt8Y


## introduction
Island is a game developed on Unity. In the game, the player will start having an empty island which will be shown on the screen. By collecting **physical cards** of different plants and scanning them by camera, the player can generate an augmented **reality (AR) model**. Then, the player can select the corresponding area to grow plants and click on the plants to unlock five different animals.Through the interaction with plants, players will gradually unlock and discover various animals, increasing the fun and exploration of the game.

After different plants are planted, different animals will be created with them. We hope that this game can make people pay attention to environmental protection and the protection of ecological diversity.

Island combines elements of placement games, biodiversity and augmented reality to provide players with a unique experience. Players will collect and **exchange cards with other players** in real world to eventually unlock all animals.

## The technologies we use
Unity

Blender

Adobe Illustrator

Ar SDK: Vuforia

## Game Flow
```mermaid
flowchart  TD;
    A[开始游戏] --> B(扫描其他卡牌)
    A -->S(在真实世界中交换卡牌)
    S -->B 
    S -->E 
    B --> D(按住“View”（预览）按钮)
    A -->E(扫描雪松（稀有）卡牌)
    B -->G(按住”Plant“（种植）按钮)
    G --点击-->C(在对应区域种植)
    C -->H(显示不同动物)
    E -->J(按住“View”（预览）按钮)
    J -->Q[显示动画]
    D -->R[显示动画]
    E -->K(按住”Plant“（种植）按钮)
    K --点击-->N(在雪地上种植)
    N -->L{蘑菇（稀有）是否被种植}
    L --是-->M(猴子出现)
    L --否-->O[弹出提示]
    M -->P[完成成就]
    H -->P
```

## What I did in this Project
### 1.Modeling in blender

![Island](https://github.com/zhangxiangna/CreativeMakingFinalProject/raw/main/image/Island.png "Island")

### 2.Making physical cards

![Mushroom](https://github.com/zhangxiangna/CreativeMakingFinalProject/raw/main/image/Mushroom.jpg "Mushroom")
![retree4](https://github.com/zhangxiangna/CreativeMakingFinalProject/raw/main/image/redtree4.jpg "redtree4")

### 3.Production of AR effect
**Previous Plans:**
Before, our plan was to scan the poster, project an augmented reality island, and then use the mouse to click on the island to generate a tree at the corresponding location, and click on the tree again to possibly have animals appear. And there will be some virtual buttons on the poster to interact with the island.
![ArIsland](https://github.com/zhangxiangna/CreativeMakingFinalProject/raw/main/image/ArIsland.png "ArIsland")

I mainly use two functions in augmented reality, the first one is to generate an augmented reality island by recognizing pictures in reality.
![Effect](https://github.com/zhangxiangna/CreativeMakingFinalProject/raw/main/image/Effect.png "Effect")

The second feature is the virtual button function, which interacts with the augmented reality island on the screen by overlaying a part of the picture in reality in order to let players feel different ways of interaction.Here is an excerpt of the code.
```ruby
public class BtnMouse : MonoBehaviour
{
    public int MouseZhuangtai = 0;
    private VirtualButtonBehaviour[] buttons;

    void Start()
    {
        buttons = this.GetComponentsInChildren<VirtualButtonBehaviour>();

        for (int i = 0; i < buttons.Length; i++)
        {
            if (buttons[i].VirtualButtonName == "Grass")
            {
                buttons[i].RegisterOnButtonPressed(PressGrass);
            }
            else if (buttons[i].VirtualButtonName == "Tree")
            {
                buttons[i].RegisterOnButtonPressed(PressTree);
        }
    }

    private void PressGrass(VirtualButtonBehaviour obj)
    {
        MouseGrass();
        Debug.Log(obj.VirtualButtonName + " pressed");
    }

    private void PressTree(VirtualButtonBehaviour obj)
    {
        MouseTree();
        Debug.Log(obj.VirtualButtonName + " pressed");
    }


    private void MouseGrass()
    {
        MouseZhuangtai = 2;
    }

    private void MouseTree()
    {
        MouseZhuangtai = 1;
    }
    }
    }
```


### Final Plan ###

But after a group discussion, we decided that this did not look well. Eventually we changed in order to scan realistic physical cards to unlock different plants, planted on the screen of the island, and thus unlock different animals.

As well as this is the final result, the complete show in the video.
![image](https://github.com/zhangxiangna/CreativeMakingFinalProject/raw/main/image/2023-06-12.png "2023-6-12")

Here is an excerpt of the code.This code is designed to implement virtual buttons in Unity, triggering different behaviors depending on the button press, such as playing particle effects, instantiating objects, loading scenes, etc.

```ruby

using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Vuforia;
using UnityEngine.SceneManagement;
public class BtnVisual_Sand : MonoBehaviour
{
    private VirtualButtonBehaviour[] buttons;
    public Vector3 TreePos;
    public GameObject Turtle;
    public GameObject SandTree;
    private Grow grow;
    void Start()
    {
        buttons = this.GetComponentsInChildren<VirtualButtonBehaviour>();
        grow = SandTree.GetComponent<Grow>();
        for (int i = 0; i < buttons.Length; i++)
        {
            if (buttons[i].VirtualButtonName == "SandView")
            {
                buttons[i].RegisterOnButtonPressed(SandView);
            }
            else if (buttons[i].VirtualButtonName == "Plant")
            {
                buttons[i].RegisterOnButtonPressed(Plant);
            }
        }       
    }

    private void SandView(VirtualButtonBehaviour obj)
    {
        SandView();
        Debug.Log(obj.VirtualButtonName + " pressed");
    }
    private void Plant(VirtualButtonBehaviour obj)
    {
        Plant();
        Debug.Log(obj.VirtualButtonName + " pressed");
    }

    private void SandView()
    {
        grow.PlayParticleEffect();
        GameObject[] animals = GameObject.FindGameObjectsWithTag("Animal");
        if (animals.Length > 0)
        {
            Debug.Log("存在标记为'Animal'的对象");
        }
        else
        {
            GameObject turtleInstance = Instantiate(Turtle, SandTree.transform.position, Quaternion.identity);
            TurtleController turtleController = turtleInstance.GetComponent<TurtleController>();
            TreePos = SandTree.transform.position;
            turtleController.SetCenterPoint(TreePos);
        }
    }
    private void Plant()
    {
        SceneManager.LoadScene(0);
    }
}
```

