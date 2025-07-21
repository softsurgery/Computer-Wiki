```js
int min(int a, int b) {
    return a < b ? a : b;
}

int maxArea(int* height, int heightSize) {
    int left = 0, right = heightSize - 1;
    int maxV = 0;

    while (left < right) {
        int h = min(height[left], height[right]);
        int width = right - left;
        int area = h * width;
        if (area > maxV) maxV = area;

        // Move the pointer pointing to the shorter line
        if (height[left] < height[right])
            left++;
        else
            right--;
    }

    return maxV;
}
```