---
layout: default
title: Add custom commands
nav_order: 1
---

# Blutilities

{: .note }
Comming soon

# C++ functions

Any C++ exposed `UFUNCTION` can be marked with the `QuickActionEntry` meta tag and it will be automatically picked up by the quick menu. It doesn't matter if your function is blueprint exposed as well.

Example usage:
```c++
// Example tooltip
UFUNCTION(meta = (QuickActionEntry))
void ExampleFunction();
```

By using this method, you can add entries to the quick menu *without* adding a hard depedency on the plugin. This way your code will still work even after the plugin is removed *with no additional changes*.

{: .warning }
Currently only functions with no input arguments are supported

# Custom

If you have special requirements, you can implement a new QuickMenu extension to create custom commands and add as many entries to the menu as you need.

Add the `"QuickMenu"` depedency to your `.Build.cs` file:
```c++
PrivateDependencyModuleNames.AddRange( new string[] { "QuickMenu" });
```

Then create a new class inheriting from `UQuickMenuExtension`:
```c++
/**
 * Adds my cool quick menu entries
 */
UCLASS()
class UMyCoolExtension : public UQuickMenuExtension
{
	GENERATED_BODY()

	virtual TArray<TSharedPtr<FQuickCommandEntry>> GetCommands(const FToolMenuContext& Context) override;
};
```

then inside the implementation of `GetCommands` you can add your commands:
```c++
TArray<TSharedPtr<FQuickCommandEntry>> UMyCoolExtension::GetCommands(const FToolMenuContext& Context)
{
	TArray<TSharedPtr<FQuickCommandEntry>> OutCommands;

	const TSharedPtr<FQuickCommandEntry> MyCoolEntry = MakeShared<FQuickCommandEntry>();
	MyCoolEntry->Title = INVTEXT("My Cool Function Title");
	MyCoolEntry->Tooltip = INVTEXT("My Cool Function Tooltip");
	MyCoolEntry->Icon = FSlateIcon(FAppStyle::GetAppStyleSetName(), "MyCoolFunctionIcon");
	MyCoolEntry->ExecuteCallback = FSimpleDelegate::CreateLambda(
		[=]()
		{
			UE_LOG(LogTemp, Warning, TEXT("My Cool Function was executed"));
		}
	);

	OutCommands.Add(MyCoolEntry);
	return OutCommands;
}
```