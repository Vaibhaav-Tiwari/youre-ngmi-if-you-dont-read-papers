# The classic paper that introduced backpropagation

This research paper introduces backpropagation which we widely use today. The main goal is to make the network's output as close as possible to the desired output. This method is especially important because it helps networks **automatically discover "internal representations" or "features"** within the data, which are not directly given in the input or output and are very difficult to program manually.

The core idea is called **back-propagation**, and it's a clever way to figure out how much each hidden part of the network contributed to any errors in the final output, even though we don't know what the ideal output for those hidden parts should be.

## How the Learning Procedure Works

The process involves a network of interconnected "units" (like simplified neurons) and a two-step learning cycle:

### 1. Network Structure

*   The network is made of several layers: an **input layer**, one or more **hidden layers** in the middle, and an **output layer**.
*   Units in one layer are connected to units in the layers above and below them, and sometimes even skip layers.
*   Each connection between units has a **weight**, which determines the strength of that connection.

### 2. Forward Pass (Calculating Outputs)

*   **Input to a unit**: For any unit $j$, its total input ($x_j$) is calculated by taking the outputs ($y_i$) from all connected units $i$ in the previous layer and multiplying them by their respective connection weights ($w_{ji}$), then summing these up.
    *   **Formula**: $$
        x_j = \sum_i y_i \cdot w_{ji}
        $$
*   **Output of a unit**: This calculated input ($x_j$) is then fed into a **non-linear function** (specifically, the **logistic function**) to produce the unit's output ($y_j$). This function squashes the output into a range between 0 and 1.
    *   **Formula**: $$
        y_j = \frac{1}{1 + e^{-x_j}}
        $$
*   Each unit also has a "bias," which acts like a weight from a unit that always outputs 1.

### 3. Backward Pass (Learning from Errors - Back-propagation)

This is where the network learns by adjusting its weights based on errors.

*   **Error Calculation**: First, the network calculates the **difference between its actual output and the desired output** for a given input. The goal is to minimize the **sum of squared errors** across all output units.
*   **Propagating Error Backwards**:
    *   The learning starts by calculating how much each connection weight contributed to the error, beginning with the **output layer**.
    *   For **output units**, the exact contribution of each weight to the error is directly calculated.
    *   For **hidden units**, it's more complex because we don't know what their "correct" outputs should be. The algorithm cleverly **propagates the error information backwards** from the output layer, through the hidden layers. It essentially figures out how much changing the input to a hidden unit would affect the overall error, by summing up the error signals from the units it connects to in the next layer.
    *   This process allows the algorithm to determine how much the overall error changes with respect to the total input of any unit ($\frac{\partial E}{\partial x_j}$).
    *   Once $\frac{\partial E}{\partial x_j}$ is known for a unit, the contribution of any incoming weight ($w_{ji}$) to that unit's input can be determined.
        *   **Formula**: $$
            \frac{\partial E}{\partial w_{ji}} = \frac{\partial E}{\partial x_j} \cdot y_i
            $$

### 4. Weight Adjustment (Mathematics of Learning)

After calculating these error contributions, the network adjusts its weights to reduce the error using a process called **gradient descent**.

*   **Basic Weight Update**: Each weight ($w_{ji}$) is adjusted by subtracting a small amount proportional to its error contribution. This moves the weight in the direction that decreases the error.
    *   **Formula**: $$
        w_{ji}^{\text{new}} = w_{ji}^{\text{old}} - \varepsilon \cdot \frac{\partial E}{\partial w_{ji}}
        $$
    *   Here, **$\varepsilon$ (epsilon) is the learning rate**, which controls how big each adjustment step is. A larger learning rate means faster, but potentially unstable, learning.
*   **Adding Momentum**: To help the learning process speed up and avoid getting stuck in minor "dips" (local minima) in the error landscape, a **momentum term ($\alpha$)** can be added. This means that the current weight change is influenced by the previous weight changes, giving it "momentum" to continue in a consistent direction.
    *   **Formula**: $$
        \Delta w_{ji}(t) = -\varepsilon \cdot \frac{\partial E}{\partial w_{ji}}(t) + \alpha \cdot \Delta w_{ji}(t-1)
        $$
    *   The new weight is then $$
        w_{ji}(t+1) = w_{ji}(t) + \Delta w_{ji}(t)
        $$

## Challenges and Considerations

While powerful, the paper notes a few challenges with this procedure:

*   It can be **computationally intensive and slow** for very large networks.
*   The network might sometimes get stuck in **local minima**, meaning it finds a good solution but not necessarily the absolute best possible one.
*   The paper also points out that, in its current form, this learning procedure is not presented as a biologically plausible model for how brains learn.