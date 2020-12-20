# DyingLight
- Not Latest Update Steam

## Structs
```cpp

class entities
{
public:
	void *Validation; //0x0000
	void *Validation1; //0x0008
	void *Validation2; //0x0010
	void *Validation3; //0x0018
	int32_t type; //0x0020
	int8_t type2; //0x0024
	char _0x242[3];
	char pad_0028[148]; //0x0028
	vec3 origin; //0x00BC
	char pad_00C8[4]; //0x00C8
	float viewZ; //0x00CC
	char pad_00D0[832]; //0x00D0


}; //Size: 0x1960
```

## Entity Hook
```cpp
typedef __int64(__fastcall* sub_3849A0_t)(float *a1, entities* a2, unsigned int a3, const void *a4, int a5, float a6, __int64 a7);
sub_3849A0_t sub_3849A0 = sub_3849A0_t(Utils.Module.ResolveImport(L"engine_x64_rwdi.dll") + 0x382830);
__int64 sub_3849A0_Hook(float *a1, entities* a2, unsigned int a3, const void *a4, int a5, float a6, __int64 a7)

```

## WorldToScreen
```cpp
viewMatrix: Utils.Module.ResolveImport(L"engine_x64_rwdi.dll") + 0xa91dd0

bool WorldToScreen(vec3 pos, vec2 &screen, float matrix[16], int windowWidth, int windowHeight)
{
	//Matrix-vector Product, multiplying world(eye) coordinates by projection matrix = clipCoords
	vec4 clipCoords;
	clipCoords.x = pos.x*matrix[0] + pos.y*matrix[1] + pos.z*matrix[2] + matrix[3];
	clipCoords.y = pos.x*matrix[4] + pos.y*matrix[5] + pos.z*matrix[6] + matrix[7];
	clipCoords.z = pos.x*matrix[8] + pos.y*matrix[9] + pos.z*matrix[10] + matrix[11];
	clipCoords.w = pos.x*matrix[12] + pos.y*matrix[13] + pos.z*matrix[14] + matrix[15];

	if (clipCoords.w < 0.1f)
		return false;

	//perspective division, dividing by clip.W = Normalized Device Coordinates
	vec3 NDC;
	NDC.x = clipCoords.x / clipCoords.w;
	NDC.y = clipCoords.y / clipCoords.w;
	NDC.z = clipCoords.z / clipCoords.w;

	screen.x = (windowWidth / 2 * NDC.x) + (NDC.x + windowWidth / 2);
	screen.y = -(windowHeight / 2 * NDC.y) + (NDC.y + windowHeight / 2);
	return true;
}
```
