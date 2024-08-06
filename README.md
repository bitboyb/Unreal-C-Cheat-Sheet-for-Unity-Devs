# Unreal C++ Cheat Sheet for Unity Devs

## Introduction

Welcome to the Unreal C++ Cheat Sheet for Unity Devs!

Transitioning from Unity to Unreal Engine can be an exciting journey, and this guide is crafted to ease that transition by highlighting key differences and similarities between the two environments. Unreal Engine and Unity both offer robust tools for game development, but their syntax and methodologies can vary significantly. This cheat sheet is designed to bridge that gap by offering a clear comparison of core concepts and practical examples in Unreal C++.

In this guide, you'll find:

**Syntax & Key Differences:** A detailed overview of the syntax and operators in Unreal C++. This section will help you understand the unique features and conventions of Unreal Engine, such as the use of macros, scope resolution operators, and smart pointers.

**Core Unreal Classes:** An overview of fundamental classes in Unreal Engine, such as `AActor`, `APawn`, and `ACharacter`, along with practical examples showing how to use them effectively. This section will also illustrate how these classes compare to their Unity counterparts.

**Function Examples:** Examples of common functionalities in Unreal C++, including variable declarations, function implementations, and event handling. This will help you get acquainted with Unreal’s coding style and practices.

**Unreal Engine-Specific Features:** Insights into unique features of Unreal Engine, including macros like `UCLASS`, `UPROPERTY`, and `UFUNCTION`, as well as its approach to delegates, shaders, and other advanced programming concepts.

Additionally, at the end of the cheat sheet, you'll find some tips for getting started with Unreal C++, ensuring a smoother transition and helping you leverage Unreal Engine's powerful features effectively.

---

## Syntax & Key Differences

### **Keywords**

#### `this`
In C++, `this` is a pointer to the current instance of a class. In C#, `this` is used similarly but is not a pointer.

#### `const`
C++ uses `const` to define constant variables and methods that do not modify class state. In C#, `const` is also used for constants, but with different rules.
``` cpp
// Const Variables: Define values that cannot be changed after initialization.

const int MaxValue = 100;

//Const Pointers: Can point to mutable data but cannot modify the data through the pointer.

int Value = 10;
const int* Ptr = &Value; // Pointer to const int

// Const Methods: Indicate that the method does not alter the object's state.

class MyClass {
public:
    void PrintValue() const; // This method cannot modify class members
};

```

#### `decltype`
This is specific to C++ and does not have a direct equivalent in C#, it used to determine the type of an expression at compile time.
``` cpp
int MyInteger = 10;
decltype(MyInteger) NewVariable = 20; // NewVariable is of type int
```

---

### **Operators, Pointers & Preprocessor Directives**

#### `::` (Scope Resolution Operator)
Used to access static members of a class or to specify namespaces. For example, `AActor::StaticClass()` is used to access the static class function of `AActor`.

```cpp
UClass* MyClass = AActor::StaticClass();
```

#### `*` (Pointer Dereference)
In Unreal Engine C++, pointers are frequently used. The `*` operator is used to dereference a pointer to access the value it points to.

```cpp
AActor* MyActor = GetWorld()->SpawnActor<AActor>();
MyActor->DoSomething(); // Dereferencing pointer to call method
```

#### `&` (Address-of Operator)
Used to obtain the address of a variable. For example, `&MyVariable` gives the memory address of `MyVariable`.

```cpp
int32 MyVariable = 10;
int32* MyVariablePtr = &MyVariable; // Getting address of MyVariable
```

#### `->` (Member Access Operator for Pointers)
Used to access members of an object through a pointer. For example, `MyActor->GetName()` accesses the `GetName` method of the `MyActor` pointer.

```cpp
AActor* MyActor = GetWorld()->SpawnActor<AActor>();
FString ActorName = MyActor->GetName(); // Accessing method through pointer
```

#### `TSharedPtr<>`, `TWeakPtr<>`, and `TUniquePtr<>`
Smart pointers used for automatic memory management in Unreal. `TSharedPtr` provides shared ownership, `TWeakPtr` is a non-owning pointer that does not affect reference counting, and `TUniquePtr` represents unique ownership of a resource.

```cpp
TSharedPtr<AActor> SharedActor = MakeShareable(new AActor());
TWeakPtr<AActor> WeakActor = SharedActor;
TUniquePtr<AActor> UniqueActor = MakeUnique<AActor>();
```

#### `#` (Preprocessor Directive)
A preprocessor directive is an instruction in C++ code that is processed by the preprocessor before compilation, used to control file inclusion, macro definition, and conditional compilation. For example, `#include "MyHeader.h"` includes the header file named `MyHeader.h`.

```cpp
#include "MyHeader.h"
```

---

### **Variable Types**

#### `int32`
A 32-bit signed integer.

```cpp
int32 MyInteger = 42;
```

#### `float`
A single-precision floating-point number.

```cpp
float MyFloat = 3.14f;
```

#### `double`
A double-precision floating-point number.

```cpp
double MyDouble = 3.14159265358979;
```

#### `bool`
A Boolean value that can be `true` or `false`.

```cpp
bool bIsTrue = true;
```

#### `FString`
Unreal's string class used for text manipulation.

```cpp
FString MyString = TEXT("Hello, Unreal!");
```

#### `FText`
A text class used for localization and rich text formatting.

```cpp
FText MyText = FText::FromString(TEXT("Localized text"));
```

#### `TArray<T>`
A dynamic array that can hold elements of type `T`.

```cpp
TArray<int32> MyArray;
MyArray.Add(1);
MyArray.Add(2);
```

#### `TMap<KeyType, ValueType>`
A dictionary that maps keys of type `KeyType` to values of type `ValueType`.

```cpp
TMap<FString, int32> MyMap;
MyMap.Add(TEXT("Key1"), 100);
MyMap.Add(TEXT("Key2"), 200);
```

#### `UObject*`
A pointer to an `UObject`, the base class for all objects in Unreal's reflection system.

```cpp
UObject* MyObject = nullptr;
```

#### `AActor*`
A pointer to an `AActor`, which is the base class for all actors in Unreal.

```cpp
AActor* MyActor = nullptr;
```

#### `UStaticMeshComponent*`
A pointer to a `UStaticMeshComponent`, used for rendering static meshes.

```cpp
UStaticMeshComponent* MyMeshComponent = nullptr;
```

#### `FVector`
A 3D vector used for positions, directions, and velocities.

```cpp
FVector MyVector(0.0f, 0.0f, 0.0f);
```

#### `FRotator`
A structure representing rotations in pitch, yaw, and roll.

```cpp
FRotator MyRotator(0.0f, 0.0f, 0.0f);
```

#### `FQuat`
A quaternion used for representing rotations in 3D space.

```cpp
FQuat MyQuat = FQuat::Identity;
```

---

### **Macros**

#### `UCLASS()`

Declares a class as an Unreal class, enabling reflection and other engine features.

```cpp
// MyUnrealActor.h
UCLASS()
class MYGAME_API AMyUnrealActor : public AActor
{
    GENERATED_BODY()

public:
    AMyUnrealActor();
    virtual void BeginPlay() override;
};

// MyUnrealActor.cpp
#include "MyUnrealActor.h"

AMyUnrealActor::AMyUnrealActor()
{
    PrimaryActorTick.bCanEverTick = true;
}

void AMyUnrealActor::BeginPlay()
{
    Super::BeginPlay();
    UE_LOG(LogTemp, Warning, TEXT("Hello, Unreal!"));
}
```

---

#### `UPROPERTY()`

Exposes a class member variable to Unreal’s reflection system and optionally to the editor and Blueprints.

```cpp
// MyUnrealActor.h
UCLASS()
class MYGAME_API AMyUnrealActor : public AActor
{
    GENERATED_BODY()

public:
    AMyUnrealActor();

    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = "MyCategory")
    int32 MyInteger;

    UPROPERTY(VisibleAnywhere, BlueprintReadOnly, Category = "MyCategory")
    FString MyString;
};

// MyUnrealActor.cpp
#include "MyUnrealActor.h"

AMyUnrealActor::AMyUnrealActor()
{
    MyInteger = 42;
    MyString = TEXT("Hello, Unreal!");
}
```

---

#### `UFUNCTION()`

Declares a function as part of Unreal’s reflection system, enabling it to be called from Blueprints or exposed to other parts of the engine.

```cpp
// MyUnrealActor.h
UCLASS()
class MYGAME_API AMyUnrealActor : public AActor
{
    GENERATED_BODY()

public:
    AMyUnrealActor();

    UFUNCTION(BlueprintCallable, Category = "MyCategory")
    void MyFunction();
};

// MyUnrealActor.cpp
#include "MyUnrealActor.h"

AMyUnrealActor::AMyUnrealActor()
{
}

void AMyUnrealActor::MyFunction()
{
    UE_LOG(LogTemp, Warning, TEXT("Function called!"));
}
```

---

#### `GENERATED_BODY()`

A macro that generates boilerplate code required for Unreal’s reflection system. It should be placed in the class body.

```cpp
// MyUnrealActor.h
UCLASS()
class MYGAME_API AMyUnrealActor : public AActor
{
    GENERATED_BODY()

public:
    AMyUnrealActor();
    virtual void BeginPlay() override;
};

// MyUnrealActor.cpp
#include "MyUnrealActor.h"

AMyUnrealActor::AMyUnrealActor()
{
    PrimaryActorTick.bCanEverTick = true;
}

void AMyUnrealActor::BeginPlay()
{
    Super::BeginPlay();
    UE_LOG(LogTemp, Warning, TEXT("Hello, Unreal!"));
}
```

---

#### `USTRUCT()`

Declares a structure as an Unreal type, enabling reflection and integration with the engine’s systems.

```cpp
// MyStruct.h
USTRUCT(BlueprintType)
struct FMyStruct
{
    GENERATED_BODY()

    UPROPERTY(EditAnywhere, BlueprintReadWrite)
    int32 MyValue;
};

// MyStruct.cpp
#include "MyStruct.h"

```

---

#### `UENUM()`

Declares an enumeration as an Unreal type, allowing it to be used in Blueprints and exposed to the editor.

```cpp
// MyEnum.h
UENUM(BlueprintType)
enum class EMyEnum : uint8
{
    Value1 UMETA(DisplayName = "Value 1"),
    Value2 UMETA(DisplayName = "Value 2")
};

// MyEnum.cpp
#include "MyEnum.h"

```

---

#### `UINTERFACE()`

Declares an interface class for Unreal’s reflection system, which can be implemented by other classes.

```cpp
// MyInterface.h
UINTERFACE(BlueprintType)
class UMyInterface : public UInterface
{
    GENERATED_BODY()
};

class IMyInterface
{
    GENERATED_BODY()

public:
    UFUNCTION(BlueprintCallable, Category = "MyInterface")
    void MyInterfaceFunction();
};

// MyInterface.cpp
#include "MyInterface.h"
```

---

#### `IMPLEMENT_PRIMARY_GAME_MODULE()`

Defines the primary game module for the project. This is usually located in the `.cpp` file of the module.

```cpp
// MyGame.cpp
#include "MyGame.h"
#include "Modules/ModuleManager.h"

IMPLEMENT_PRIMARY_GAME_MODULE(FDefaultGameModuleImpl, MyGame, "MyGame");
```

---

#### `IMPLEMENT_MODULE()`

Implements a module class for plugin or module systems.

```cpp
// MyModule.cpp
#include "MyModule.h"
#include "Modules/ModuleManager.h"

IMPLEMENT_MODULE(FMyModule, MyModuleName);
```

---

#### `DEFINE_LOG_CATEGORY()`

Defines a logging category for debugging and logging.

```cpp
// MyLogCategory.h
#pragma once

#include "CoreMinimal.h"

DEFINE_LOG_CATEGORY(LogMyCategory);
```

```cpp
// MyLogCategory.cpp
#include "MyLogCategory.h"

```

---


## Cheat Sheet

### **Basics of Unreal C++**

#### **Project Structure**

- **Unity**: Scripts are usually located in the `Assets` folder.
- **Unreal**: C++ classes are located in the `Source` folder, with separate header (.h) and source (.cpp) files.

**Unity Example**:
```csharp
public class MyUnityScript : MonoBehaviour
{
    void Start()
    {
        Debug.Log("Hello, Unity!");
    }
}
```

**Unreal Example**:
```cpp
// MyUnrealActor.h
UCLASS()
class MYGAME_API AMyUnrealActor : public AActor
{
    GENERATED_BODY()

public:
    virtual void BeginPlay() override;
};

// MyUnrealActor.cpp
#include "MyUnrealActor.h"

void AMyUnrealActor::BeginPlay()
{
    Super::BeginPlay();
    UE_LOG(LogTemp, Warning, TEXT("Hello, Unreal!"));
}
```

#### **Classes and Inheritance**

- **Unity**: Inherits from `MonoBehaviour`.
- **Unreal**: Inherits from Unreal-specific classes like `AActor`, `APawn`, `ACharacter`.

**Unity Example**:
```csharp
public class MyUnityCharacter : MonoBehaviour
{
    void Update()
    {
        // Character logic
    }
}
```

**Unreal Example**:
```cpp
// MyUnrealCharacter.h
UCLASS()
class MYGAME_API AMyUnrealCharacter : public ACharacter
{
    GENERATED_BODY()

public:
    virtual void Tick(float DeltaTime) override;
};

// MyUnrealCharacter.cpp
#include "MyUnrealCharacter.h"

void AMyUnrealCharacter::Tick(float DeltaTime)
{
    Super::Tick(DeltaTime);
    // Character logic
}
```

#### **Basic Actor Examples**

**1. `AActor`**
- **Description**: The base class for all actors in Unreal Engine. It provides the fundamental properties and behaviors for any object that can be placed in a level. Custom actors are derived from this class to implement game-specific functionality.
- **Usage**: Used for creating any in-game object that needs to be placed in the level, such as items, lights, or cameras.

**Example**:

```cpp
// MyActor.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "MyActor.generated.h"

UCLASS()
class MYGAME_API AMyActor : public AActor
{
    GENERATED_BODY()

public:
    AMyActor();

    // Function to set the actor's location
    void SetActorLocation(FVector NewLocation);
};

// MyActor.cpp
#include "MyActor.h"

AMyActor::AMyActor()
{
    PrimaryActorTick.bCanEverTick = true;
}

void AMyActor::SetActorLocation(FVector NewLocation)
{
    // Set the actor's location in the world
    SetActorLocation(NewLocation);
}
```

**2. `APawn`**
- **Description**: A subclass of `AActor` designed to represent entities that can be controlled by players or AI. Pawns are usually the base class for characters or any other movable entities in the game.
- **Usage**: Typically used for controllable entities in the game, such as player characters or NPCs that need movement and input handling.

**Example**:

```cpp
// MyPawn.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Pawn.h"
#include "MyPawn.generated.h"

UCLASS()
class MYGAME_API AMyPawn : public APawn
{
    GENERATED_BODY()

public:
    // Function to apply movement input to the pawn
    void MoveForward(float Value);
};

// MyPawn.cpp
#include "MyPawn.h"

void AMyPawn::MoveForward(float Value)
{
    // Apply movement input in the forward direction
    AddMovementInput(GetActorForwardVector(), Value);
}
```

**3. `ACharacter`**
- **Description**: A specialized subclass of `APawn` that adds functionality for characters, including movement, jumping, and animation. It includes a `CharacterMovementComponent` for handling complex movement mechanics.
- **Usage**: Used as the base class for player characters or NPCs that require advanced movement and animation features.

**Example**:

```cpp
// MyCharacter.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Character.h"
#include "MyCharacter.generated.h"

UCLASS()
class MYGAME_API AMyCharacter : public ACharacter
{
    GENERATED_BODY()

public:
    // Function to make the character jump
    void Jump();
};

// MyCharacter.cpp
#include "MyCharacter.h"

void AMyCharacter::Jump()
{
    // Call the built-in jump function
    Super::Jump();
}
```

**4. `APlayerController`**
- **Description**: Manages player input and controls the interaction between the player and their `Pawn` or `Character`. It handles player-specific actions, such as movement, camera control, and UI interactions.
- **Usage**: Used to implement custom player input logic and control the player's interaction with the game world.

**Example**:

```cpp
// MyPlayerController.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/PlayerController.h"
#include "MyPlayerController.generated.h"

UCLASS()
class MYGAME_API AMyPlayerController : public APlayerController
{
    GENERATED_BODY()

public:
    // Function to handle custom input action
    void OnJumpPressed();
};

// MyPlayerController.cpp
#include "MyPlayerController.h"
#include "MyCharacter.h"

void AMyPlayerController::OnJumpPressed()
{
    if (GetPawn())
    {
        // Make the controlled pawn jump
        ACharacter* Character = Cast<ACharacter>(GetPawn());
        if (Character)
        {
            Character->Jump();
        }
    }
}
```

**5. `AGameModeBase`**
- **Description**: Defines the rules and flow of the game. It controls aspects like spawning players, managing scoring, and handling game state. It is a central part of game logic and flow.
- **Usage**: Used to set up game-specific rules, manage game state, and control how players enter and exit the game.

**Example**:

```cpp
// MyGameMode.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/GameModeBase.h"
#include "MyGameMode.generated.h"

UCLASS()
class MYGAME_API AMyGameMode : public AGameModeBase
{
    GENERATED_BODY()

public:
    // Function to start a new game
    void StartNewGame();
};

// MyGameMode.cpp
#include "MyGameMode.h"

void AMyGameMode::StartNewGame()
{
    // Restart The Game
    RestartGame();
}
```

**6. `AGameStateBase`**
- **Description**: Holds and manages the game state that is relevant to all players. It is often used to store information such as scores, game status, and other global game data.
- **Usage**: Used to store and replicate game-wide information that needs to be available to all players, such as scoreboards or round timers.

**Example**:

```cpp
// MyGameState.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/GameStateBase.h"
#include "MyGameState.generated.h"

UCLASS()
class MYGAME_API AMyGameState : public AGameStateBase
{
    GENERATED_BODY()

public:
    // Function to update the game score
    void UpdateScore(int32 NewScore);

private:
    // The current score
    UPROPERTY()
    int32 Score;
};

// MyGameState.cpp
#include "MyGameState.h"

void AMyGameState::UpdateScore(int32 NewScore)
{
    // Update the score and notify clients
    Score = NewScore;
    OnScoreUpdated.Broadcast(Score);
}
```

**7. `AHUD`**
- **Description**: Manages the on-screen display and user interface for the player. It handles rendering HUD elements such as health bars, ammo counts, and other UI components.
- **Usage**: Used to create and manage custom HUDs and UI elements for displaying game information to the player.

**Example**:

```cpp
// MyHUD.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/HUD.h"
#include "MyHUD.generated.h"

UCLASS()
class MYGAME_API AMyHUD : public AHUD
{
    GENERATED_BODY()

public:
    // Function to draw a health bar on the HUD
    void DrawHealthBar(float Health, float MaxHealth);
};

// MyHUD.cpp
#include "MyHUD.h"

void AMyHUD::DrawHealthBar(float Health, float MaxHealth)
{
    // Draw the health bar on the screen
    DrawRect(FLinearColor::Red, 10, 10, Health / MaxHealth * 200, 20);
}
```

**8. `ALight`**
- **Description**: A base class for various types of lights in the game world, such as point lights, spotlights, and directional lights. It provides properties and methods for controlling light intensity, color, and other settings.
- **Usage**: Used to create and manage different types of lighting in the scene, affecting how objects are illuminated.

**Example**:

```cpp
// MyLight.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "Components/PointLightComponent.h"
#include "MyLight.generated.h"

UCLASS()
class MYGAME_API AMyLight : public AActor
{
    GENERATED_BODY()

public:
    // Function to adjust the light intensity
    void SetLightIntensity(float NewIntensity);

private:
    // The light component
    UPROPERTY(VisibleAnywhere)
    UPointLightComponent* LightComponent;
};

// MyLight.cpp
#include "MyLight.h"

void AMyLight::SetLightIntensity(float NewIntensity)
{
    // Set the light intensity
    if (LightComponent)
    {
        LightComponent->SetIntensity(NewIntensity);
    }
}
```

**9. `ACameraActor`**
- **Description**: Represents a camera in the scene. It defines the camera’s position, rotation, and other settings related to rendering the view from the camera.
- **Usage**: Used to place and manage cameras in the level, including configuring camera angles and settings for different perspectives.

**Example**:

```cpp
// MyCameraActor.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "Camera/CameraComponent.h"
#include "MyCameraActor.generated.h"

UCLASS()
class MYGAME_API AMyCameraActor : public AActor
{
    GENERATED_BODY()

public:
    // Function to set the camera's field of view
    void SetFieldOfView(float NewFOV);

private:
    // The camera component
    UPROPERTY(VisibleAnywhere)
    UCameraComponent* CameraComponent;
};

// MyCameraActor.cpp
#include "MyCameraActor.h"

void AMyCameraActor::SetFieldOfView(float NewFOV)
{
    // Set the camera's field of view
    if (CameraComponent)
    {
        CameraComponent->SetFieldOfView(NewFOV);
    }
}
```

**10. `AStaticMeshActor`**
- **Description**: A type of `AActor` that represents static mesh objects. It allows you to place 3D models (static meshes) in the level and manipulate

 their properties.
- **Usage**: Used for placing static 3D models in the world, such as environment props, buildings, and other non-moving objects.

**Example**:

```cpp
// MyStaticMeshActor.h
#pragma once

#include "CoreMinimal.h"
#include "GameFramework/Actor.h"
#include "Components/StaticMeshComponent.h"
#include "MyStaticMeshActor.generated.h"

UCLASS()
class MYGAME_API AMyStaticMeshActor : public AActor
{
    GENERATED_BODY()

public:
    // Function to change the static mesh of the actor
    void SetMesh(UStaticMesh* NewMesh);

private:
    // The static mesh component
    UPROPERTY(VisibleAnywhere)
    UStaticMeshComponent* MeshComponent;
};

// MyStaticMeshActor.cpp
#include "MyStaticMeshActor.h"

void AMyStaticMeshActor::SetMesh(UStaticMesh* NewMesh)
{
    // Set the new static mesh for the actor
    if (MeshComponent)
    {
        MeshComponent->SetStaticMesh(NewMesh);
    }
}
```

---

#### **Macros in Unreal C++**

- **Unity**: No macros, uses attributes like `[SerializeField]`.
- **Unreal**: Uses macros like `UCLASS`, `UPROPERTY`, `UFUNCTION` to manage reflection and other features.

**Unity Example**:
```csharp
[SerializeField]
private int myVariable;
```

**Unreal Example**:
```cpp
// MyUnrealActor.h
UCLASS()
class MYGAME_API AMyUnrealActor : public AActor
{
    GENERATED_BODY()

public:
    UPROPERTY(EditAnywhere, BlueprintReadWrite, Category="Variables")
    int32 MyVariable;
};
```

---

### **Component-Based Development**

#### **Components in Unreal vs Unity**

- **Unity**: Components are attached to GameObjects.
- **Unreal**: Components are attached to Actors.

**Unity Example**:
```csharp
public class MyUnityComponent : MonoBehaviour
{
    void Start()
    {
        gameObject.AddComponent<Rigidbody>();
    }
}
```

**Unreal Example**:
```cpp
// MyUnrealActor.h
UCLASS()
class MYGAME_API AMyUnrealActor : public AActor
{
    GENERATED_BODY()

public:
    UPROPERTY(VisibleAnywhere)
    UStaticMeshComponent* MeshComponent;

    virtual void BeginPlay() override;
};

// MyUnrealActor.cpp
#include "MyUnrealActor.h"

AMyUnrealActor::AMyUnrealActor()
{
    MeshComponent = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("MeshComponent"));
    RootComponent = MeshComponent;
}

void AMyUnrealActor::BeginPlay()
{
    Super::BeginPlay();
    // Additional initialization
}
```

#### **Managing and Accessing Components**

- **Unity**: Access components using `GetComponent`.
- **Unreal**: Access components using `FindComponentByClass` or through class members.

**Unity Example**:
```csharp
void Start()
{
    Rigidbody rb = GetComponent<Rigidbody>();
}
```

**Unreal Example**:
```cpp
void AMyUnrealActor::BeginPlay()
{
    Super::BeginPlay();
    UStaticMeshComponent* MeshComp = FindComponentByClass<UStaticMeshComponent>();
}
```

---

### **Event Handling and Delegates**

#### **Unreal's Equivalent of UnityEvents**

- **Unity**: Uses `UnityEvent` for custom events.
- **Unreal**: Uses `DECLARE_DYNAMIC_MULTICAST_DELEGATE` and `BlueprintAssignable`.

**Unity Example**:
```csharp
public class MyUnityEventClass : MonoBehaviour
{
    public UnityEvent OnMyEvent;

    void Start()
    {
        OnMyEvent.Invoke();
    }
}
```

**Unreal Example**:
```cpp
// MyUnrealActor.h
UCLASS()
class MYGAME_API AMyUnrealActor : public AActor
{
    GENERATED_BODY()

public:
    DECLARE_DYNAMIC_MULTICAST_DELEGATE(FMyDelegate);

    UPROPERTY(BlueprintAssignable, Category="Events")
    FMyDelegate OnMyEvent;
};

// MyUnrealActor.cpp
#include "MyUnrealActor.h"

void AMyUnrealActor::BeginPlay()
{
    Super::BeginPlay();
    OnMyEvent.Broadcast();
}
```

#### **Custom Events and Delegates**

- **Unity**: Creating and subscribing to custom events.
- **Unreal**: Declaring and binding custom delegates.

**Unity Example**:
```csharp
public class MyUnityEventClass : MonoBehaviour
{
    public delegate void MyCustomEvent();
    public event MyCustomEvent OnEvent;

    void Start()
    {
        OnEvent?.Invoke();
    }
}
```

**Unreal Example**:
```cpp
// MyUnrealActor.h
UCLASS()
class MYGAME_API AMyUnrealActor : public AActor
{
    GENERATED_BODY()

public:
    DECLARE_DELEGATE(FMyCustomDelegate);
    FMyCustomDelegate OnEvent;

    virtual void BeginPlay() override;
};

// MyUnrealActor.cpp
#include "MyUnrealActor.h"

void AMyUnrealActor::BeginPlay()
{
    Super::BeginPlay();
    if (OnEvent.IsBound())
    {
        OnEvent.Execute();
    }
}
```

---

### **Physics and Collision Handling**

#### **Applying Forces and Impulses**

- **Unity**: Uses `Rigidbody.AddForce`.
- **Unreal**: Uses `UPrimitiveComponent::AddForce`.

**Unity Example**:
```csharp
void Start()
{
    Rigidbody rb = GetComponent<Rigidbody>();
    rb.AddForce(Vector3.up * 10f, ForceMode.Impulse);
}
```

**Unreal Example**:
```cpp
void AMyUnrealActor::BeginPlay()
{
    Super::BeginPlay();
    UStaticMeshComponent* MeshComp = FindComponentByClass<UStaticMeshComponent>();
    if (MeshComp)
    {
        MeshComp->AddImpulse(FVector(0, 0, 1000));
    }
}
```

#### **Collision Channels and Responses**

- **Unity**: Customize collision behavior using layers and tags.
- **Unreal**: Use collision channels and responses.

**Unity Example**:
```csharp
void OnCollisionEnter(Collision collision)
{
    if (collision.gameObject.tag == "Enemy")
    {
        // Collision logic
    }
}
```

**Unreal Example**:
```cpp
void AMyUnrealActor::OnComponentHit(UPrimitiveComponent* HitComponent, AActor* OtherActor, UPrimitiveComponent* OtherComponent, FVector NormalImpulse, const FHitResult& Hit)
{
    if (OtherActor->ActorHasTag("Enemy"))
    {
        // Collision logic
    }
}
```

---

### **Memory Management and Object Handling**

#### **UObjects and Garbage Collection**

- **Unity**: Memory management is mostly automatic with garbage collection.
- **Unreal**: UObjects are managed by Unreal’s garbage collector, but you have more control.

**Unity Example**:
```csharp
void Start()
{
    GameObject obj = new GameObject();
    Destroy(obj, 2f);
}
```

**Unreal Example**:
```cpp
void AMyUnrealActor::BeginPlay()
{
    Super::BeginPlay();
    UMyObject* MyObject = NewObject<UMyObject>(this);
    MyObject->AddToRoot(); // Prevents GC
    // MyObject->RemoveFromRoot(); // Allows GC
}
```

#### **Smart Pointers**

- **Unity**: Not commonly used, since C# manages memory.
- **Unreal**: Uses `TSharedPtr`, `TWeakPtr`, and `TUniquePtr` for memory management.

**Unreal Example**:
```cpp
TSharedPtr<MyClass> MySharedPointer = MakeShareable(new MyClass());
TWeakPtr<MyClass> MyWeakPointer = MySharedPointer;
TUniquePtr<MyClass> MyUniquePointer = MakeUnique<MyClass>();
```

---

### **Asset Management**

#### **Loading and Referencing Assets**

- **Unity**: Uses `Resources.Load` or drag-and-drop assignment.
- **Unreal**: Uses `LoadObject` and `StreamableManager` for asynchronous loading.

**Unity Example**:
```csharp
void Start()
{
    GameObject myPrefab = Resources.Load<GameObject>("MyPrefab");
}
```

**Unreal Example**:
```cpp
void AMyUnrealActor::BeginPlay()
{
    Super::BeginPlay();
    UStaticMesh* MyMesh = LoadObject<UStaticMesh>(nullptr, TEXT("/Game/MyMesh.MyMesh"));
}
```

#### **Soft and Hard References**

- **Unity**: References can be managed by dragging assets in the Inspector.
- **Unreal**: Uses `TSoftObjectPtr` for soft references and direct pointers

 for hard references.

**Unity Example**:
```csharp
[SerializeField]
private GameObject myPrefab;
```

**Unreal Example**:
```cpp
UPROPERTY(EditAnywhere, Category = "Assets")
TSoftObjectPtr<UStaticMesh> MyMesh;
```

---

### **Gameplay Framework**

#### **Core Gameplay Classes**

- **Unity**: `GameObject`, `MonoBehaviour`.
- **Unreal**: `AActor`, `APawn`, `ACharacter`, `APlayerController`, `AGameModeBase`.

**Unity Example**:
```csharp
public class MyUnityGameManager : MonoBehaviour
{
    void Start()
    {
        // Game logic
    }
}
```

**Unreal Example**:
```cpp
// MyUnrealGameMode.h
UCLASS()
class MYGAME_API AMyUnrealGameMode : public AGameModeBase
{
    GENERATED_BODY()

public:
    virtual void BeginPlay() override;
};

// MyUnrealGameMode.cpp
#include "MyUnrealGameMode.h"

void AMyUnrealGameMode::BeginPlay()
{
    Super::BeginPlay();
    // Game logic
}
```

---

### **Advanced Blueprint Integration**

#### **Exposing Functions and Variables to Blueprints**

- **Unity**: Public variables are exposed to the Inspector.
- **Unreal**: Uses `UPROPERTY`, `UFUNCTION` macros to expose to Blueprints.

**Unity Example**:
```csharp
public int myVariable = 5;
```

**Unreal Example**:
```cpp
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category="Variables")
int32 MyVariable = 5;
```

#### **BlueprintNativeEvent vs BlueprintImplementableEvent**

- **Unity**: All functions are callable from scripts.
- **Unreal**: Differentiates between functions that can be overridden or fully implemented in Blueprints.

**Unreal Example**:
```cpp
UFUNCTION(BlueprintNativeEvent, BlueprintCallable, Category="Events")
void MyNativeEvent();

UFUNCTION(BlueprintImplementableEvent, Category="Events")
void MyImplementableEvent();
```

---

### **Networking and Replication**

#### **Networking Basics**

- **Unity**: Uses UNet, Photon, or other third-party solutions.
- **Unreal**: Built-in networking with server-client architecture.

**Unity Example**:
```csharp
void Start()
{
    NetworkManager.StartServer();
}
```

**Unreal Example**:
```cpp
void AMyUnrealGameMode::BeginPlay()
{
    Super::BeginPlay();
    if (HasAuthority())
    {
        // Server logic
    }
}
```

#### **Replicating Variables and Functions**

- **Unity**: Synchronize variables manually with networking libraries.
- **Unreal**: Uses `UPROPERTY(Replicated)` and `UFUNCTION(Server, Reliable)`.

**Unity Example**:
```csharp
[SyncVar]
public int myVariable;
```

**Unreal Example**:
```cpp
UPROPERTY(Replicated, EditAnywhere, Category="Replication")
int32 MyVariable;
```

---

### **AI and Behavior Trees**

#### **Behavior Trees Overview**

- **Unity**: AI logic often scripted directly or using third-party tools.
- **Unreal**: Built-in Behavior Trees and Blackboards for AI.

**Unity Example**:
```csharp
void Update()
{
    // AI logic
}
```

**Unreal Example**:
```cpp
// AIController setup in Unreal, using Behavior Trees
void AMyAIController::BeginPlay()
{
    Super::BeginPlay();
    if (UseBlackboard(BlackboardAsset, BlackboardComponent))
    {
        RunBehaviorTree(BehaviorTreeAsset);
    }
}
```

---

### **UI Development with UMG**

#### **Creating and Managing UIs**

- **Unity**: Uses Unity UI (Canvas, Text, Buttons).
- **Unreal**: Uses Unreal Motion Graphics (UMG) and `UUserWidget`.

**Unity Example**:
```csharp
public class MyUnityUI : MonoBehaviour
{
    public Text myText;

    void Start()
    {
        myText.text = "Hello, Unity!";
    }
}
```

**Unreal Example**:
```cpp
// MyUnrealWidget.h
UCLASS()
class MYGAME_API UMyUnrealWidget : public UUserWidget
{
    GENERATED_BODY()

public:
    UPROPERTY(meta = (BindWidget))
    UTextBlock* MyText;

    virtual void NativeConstruct() override;
};

// MyUnrealWidget.cpp
#include "MyUnrealWidget.h"

void UMyUnrealWidget::NativeConstruct()
{
    Super::NativeConstruct();
    MyText->SetText(FText::FromString("Hello, Unreal!"));
}
```

---

### **Advanced Rendering and Shaders**

#### **Custom Shaders with HLSL**

- **Unity**: Write custom shaders using ShaderLab and HLSL.
- **Unreal**: Use the material editor or write custom HLSL shaders.

**Unity Example**:
```csharp
Shader "Custom/SimpleColorShader"
{
    Properties
    {
        _Color ("Color", Color) = (1, 0, 0, 1) // Default red color
    }
    SubShader
    {
        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            struct appdata
            {
                float4 vertex : POSITION;
            };

            struct v2f
            {
                float4 position : SV_POSITION;
            };

            float4 _Color;

            v2f vert(appdata v)
            {
                v2f o;
                o.position = UnityObjectToClipPos(v.vertex);
                return o;
            }

            float4 frag(v2f i) : SV_Target
            {
                return _Color; // Outputs the specified color (red by default)
            }
            ENDCG
        }
    }
}


```

**Unreal Example**:
```cpp
// Simple HLSL code in Unreal's Material Custom node
float4 MyCustomShader(float4 InPosition : POSITION) : SV_Target
{
    return float4(1, 0, 0, 1); // Outputs red color
}
```
---

### **Editor Scripting and Custom Tools**

#### **Editor Utility Widgets**

- **Unity**: Uses Editor scripts and custom inspectors.
- **Unreal**: Use Editor Utility Widgets to create custom tools.

**Unity Example**:
```csharp
[CustomEditor(typeof(MyUnityScript))]
public class MyUnityEditor : Editor
{
    public override void OnInspectorGUI()
    {
        // Custom editor code
    }
}
```

**Unreal Example**:
1.  Right-click in the **Content Browser** and select **Editor Utilities > Editor Utility Widget**.
    
    ![Add Editor Utility Widget asset](https://d1iv7db44yhgxn.cloudfront.net/documentation/images/97b7090a-39ee-46eb-807a-ac4b4aa68cae/ue5_1-01-add-asset.png "Add Editor Utility Widget asset")
2.  Name your Editor Utility Widget Asset. In this example, the Asset is named **TestEditorUtility**. Double-click the **Editor Utility Widget Asset** to open the Widget Blueprint for editing.
    
    ![Name your Editor Utility Widget Asset](https://d1iv7db44yhgxn.cloudfront.net/documentation/images/d17a1e04-5ce4-4c5a-b63b-572dc59943e5/ue5_1-02-name-asset.png "Name your Editor Utility Widget Asset")
3.  Edit your Widget Blueprint as needed.
    
    [![](https://d1iv7db44yhgxn.cloudfront.net/documentation/images/9ddcff54-68e4-4ef8-9a9b-900324c73ab1/ue5_1-03-edit-widget-blueprint.png)](https://d1iv7db44yhgxn.cloudfront.net/documentation/images/9ddcff54-68e4-4ef8-9a9b-900324c73ab1/ue5_1-03-edit-widget-blueprint.png)
    
4.  Right-click the **Editor Utility Widget Asset** and select **Run Editor Utility Widget** to open an Editor tab with your Editor Utility displayed. The tab is only dockable with Level Editor tabs.
    
    ![Run Editor Utility Widget](https://d1iv7db44yhgxn.cloudfront.net/documentation/images/d57162f9-b3d1-4427-80eb-bd65aa9826ee/ue5_1-04-run-editor-utility-widget.png "Run Editor Utility Widget")
5.  Once you have run the Editor Utility Widget, it appears in the Level Editor's **Tools** dropdown, under the **Editor Utility Widgets** category.
    
    ![Test Editor Utility](https://d1iv7db44yhgxn.cloudfront.net/documentation/images/80c4da60-a340-4adf-860a-337bdc45d8a5/ue5_1-05-enable-test.png "Test Editor Utility")

---

### **Project Management and Optimization**

#### **Source Control Integration**

- **Unity**: Integrates with Git, SVN, etc.
- **Unreal**: Built-in support for Perforce, Git, and others.

**Unity Example**:
```plaintext
###
# Unity folders and files
###

[Aa]ssets/AssetStoreTools*
[Bb]uild/
[Ll]ibrary/
[Ll]ocal[Cc]ache/
[Oo]bj/
[Tt]emp/
[Uu]nityGenerated/
# file on crash reports
sysinfo.txt
# Unity3D generated meta files
*.pidb.meta

###
# VS/MD solution and project files
###

[Ee]xportedObj/
*.booproj
*.csproj
*.sln
*.suo
*.svd
*.unityproj
*.user
*.userprefs
*.pidb
.DS_Store

###
# OS generated
###

.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
Icon?
ehthumbs.db
Thumbs.db
```

**Unreal Example**:
```plaintext
###
# Unreal Engine Generated Files
###

/[Bb]uild
/[Bb]uilds
/Binaries
/DerivedDataCache
/Intermediate
/Saved

###
# Visual Studio Generated Files
###

.vs
*.rsuser
*.suo
*.user
*.userosscache
*.sln.docstates
*.VC.db
*.opendb
*.opensdf
*.sdf
# Solution Files
*.sln
.vscode
**/.history
```

#### **Profiling and Optimization Tools**

- **Unity**: Uses Unity Profiler.
- **Unreal**: Uses Unreal Insights, Stat commands.

**Unity Example**:
```csharp
void Update()
{
    Profiler.BeginSample("MySample");
    // Code to profile
    Profiler.EndSample();
}
```

**Unreal Example**:
```plaintext
// In console
stat fps
stat unit
```

---

## Tips

### **Understand the Unreal Engine Paradigm**

Unreal Engine uses a class-based system similar to Unity but with its own conventions and macros. It relies on a component-based architecture where `AActor` serves as the base class. Components, such as `UStaticMeshComponent`, are used to define the behavior and properties of actors. Familiarizing yourself with this paradigm will help you adapt to Unreal Engine’s way of handling game objects.

### **Get Comfortable with Unreal’s Build System**

Unreal Engine projects are typically built using Visual Studio, so it’s important to become comfortable navigating and using the IDE for building and debugging your projects. Additionally, understanding how to configure build settings and work with Unreal’s build system, UnrealBuildTool, will streamline your development process.

### **Learn Unreal’s Macros and Reflection System**

Unreal Engine uses macros such as `UCLASS`, `UPROPERTY`, and `UFUNCTION` for class reflection and serialization. These macros are crucial for exposing properties and methods to the editor and gameplay scripts. The `GENERATED_BODY()` macro is particularly important as it generates the necessary boilerplate code for Unreal’s reflection system.

### **Familiarize Yourself with Unreal's Memory Management**

Unreal Engine employs a garbage collection system for managing memory. It’s important to understand the lifecycle of `UObject` and `AActor` to manage memory effectively and avoid leaks. Unreal Engine also uses smart pointers, including `TSharedPtr`, `TWeakPtr`, and `TUniquePtr`, for managing object lifecycles. Getting accustomed to these will help you handle memory in a more efficient manner.

### **Understand the Differences in Input Handling**

Unreal Engine features an input mapping system for handling user input. Inputs are set up in the project settings, and the `APlayerController` class is used to handle input events. Familiarizing yourself with this system and learning how to bind input actions and axes will help you adapt to Unreal Engine’s input management.

### **Explore Unreal’s Actor and Component Systems**

The `AActor` class is the base class for most objects in Unreal, and it interacts with various components to define functionality. Understanding how to use `AActor` and its components, such as `UStaticMeshComponent`, is crucial for creating dynamic and interactive game elements.

### **Get to Know Unreal’s Blueprint System**

Unreal Engine’s Blueprint system is a powerful visual scripting tool that integrates seamlessly with C++. Learn how to expose C++ classes and functions to Blueprints, allowing for rapid prototyping and visual scripting. Familiarizing yourself with Blueprint nodes and their interaction with C++ code can greatly enhance your workflow.

### **Practice Unreal’s Workflow for Asset Management**

Unreal Engine’s Content Browser is used for managing assets like textures, meshes, and materials. Learn how to use this tool effectively and manage asset references in your C++ code using utilities like `TSubclassOf` and `ConstructorHelpers`. Understanding asset management will help you keep your project organized and efficient.

### **Explore the Unreal Engine Documentation and Community**

Unreal Engine’s extensive documentation is an invaluable resource for understanding classes, functions, and best practices. Additionally, engaging with the Unreal Engine community through forums, Discord, and other platforms can provide support, resources, and shared knowledge.

### **Experiment with Unreal Engine’s Built-In Tools**

Unreal Engine offers a variety of powerful editor tools, including the Level Editor, Material Editor, and Animation Editor. Taking the time to explore and experiment with these tools will help you become more proficient in Unreal Engine and utilize its full potential in your projects.

### **Build and Test Frequently**

Regularly building and testing your code is crucial for catching issues early and ensuring that your code integrates well with the engine. Familiarize yourself with Unreal’s debugging tools, such as the Visual Studio debugger, to troubleshoot and resolve issues effectively.

---
