# FBM-model
## Theoretical Background of the Fiber Bundle Model (FBM)

The Fiber Bundle Model (FBM) is a classical model used to describe the mechanical behavior of composite systems consisting of multiple parallel fibers. It is commonly applied in the context of soil reinforcement by root systems, where the fibers represent individual plant roots. In FBM, the mechanical interaction between roots and soil is modeled as a system of fibers that can individually break when subjected to stress. The model captures how load is redistributed among the remaining fibers when some of them fail.

### Stress-Strain Relationship

Each fiber in the bundle follows a linear elastic stress-strain relationship, governed by Hooke's law:

$$
\sigma_i = E_i \varepsilon
$$

where:

- \( \sigma_i \) is the stress in the \( i \)-th fiber,
- \( E_i \) is the elastic modulus of the \( i \)-th fiber,
- \( \varepsilon \) is the applied strain (assumed uniform across all fibers).

The overall stress-strain behavior of the system depends on the collective response of all fibers, which is influenced by their individual properties and their ability to withstand stress before breaking.

### Fiber Strength Distribution

The fibers in the bundle are not identical, and their breaking strength \( S_i \) typically follows a probabilistic distribution. A common assumption is that fiber strengths are distributed according to a Weibull distribution:

$$
P(S_i \leq s) = 1 - \exp \left( -\left( \frac{s}{S_0} \right)^\beta \right)
$$

where:

- \( S_0 \) is the scale parameter, representing the characteristic strength of the fibers,
- \( \beta \) is the shape parameter, which describes the distribution of fiber strengths,
- \( P(S_i \leq s) \) is the probability that the fiber strength is less than or equal to \( s \).

The Weibull distribution allows for modeling the variability in fiber strength, which is essential for capturing the progressive failure behavior of the bundle.

### Failure Criterion

A fiber breaks when the stress it experiences exceeds its strength. Thus, the failure criterion for the \( i \)-th fiber is:

$$
\sigma_i > S_i \implies \text{Fiber \( i \) breaks}
$$

Once a fiber breaks, it can no longer carry any load, and its contribution to the overall system response is lost.

### Load Redistribution

After some fibers break, the load they carried must be redistributed among the remaining intact fibers. In the simplest form of FBM, it is assumed that the remaining fibers share the load uniformly. The total load applied to the system is redistributed among the surviving fibers, which results in an increase in stress on these fibers. The new stress in the remaining fibers is given by:

$$
\sigma_{\text{new}} = \frac{F_{\text{total}}}{N_{\text{remaining}} A}
$$

where:

- \( F_{\text{total}} \) is the total applied force,
- \( N_{\text{remaining}} \) is the number of fibers that have not yet broken,
- \( A \) is the cross-sectional area of each fiber.

This redistribution process continues as more fibers break, eventually leading to catastrophic failure of the entire system when the remaining fibers can no longer sustain the load.

### System Response and Cumulative Failure

The macroscopic stress-strain response of the fiber bundle is the cumulative result of individual fiber failures. As the strain increases, fibers begin to break in a stochastic manner, leading to a gradual reduction in the system's load-carrying capacity. The overall stress \( \sigma_{\text{total}} \) is the average stress across all fibers:

$$
\sigma_{\text{total}} = \frac{1}{N} \sum_{i=1}^{N} \sigma_i
$$

where \( N \) is the total number of fibers. The system exhibits a complex failure behavior where partial breakdowns occur before the final collapse. The stress-strain curve often exhibits a softening behavior as more fibers break, which is characteristic of many composite materials and reinforced soil systems.

## Applications of FBM in Root Reinforcement of Soil

In the context of soil reinforcement, the Fiber Bundle Model can be used to model how plant roots enhance the shear strength and overall stability of soil. The roots act as reinforcing fibers within the soil matrix, increasing the soil's ability to withstand shear stresses and prevent erosion. By incorporating fiber strength distributions and load redistribution mechanisms, the FBM provides insights into how different root architectures and densities influence soil stability under various loading conditions.

```python
import numpy as np
import matplotlib.pyplot as plt

# 设置模型参数
N_fibers = 1000  # 纤维的数量
E_fibers = np.random.uniform(100, 200, N_fibers)  # 随机设置每根纤维的弹性模量
S_0 = 150  # 强度尺度参数
beta = 3.0  # 强度分布形状参数
fiber_strength = S_0 * np.random.weibull(beta, N_fibers)  # 每根纤维的强度

# 外部加载应变
total_strain = np.linspace(0, 0.02, 100)  # 总应变变化范围
stress_response = []  # 系统的总应力响应

# 应力再分配机制：均匀分配破坏纤维后的应力
def redistribute_stress(remaining_fibers, total_load):
    return total_load / np.sum(remaining_fibers)

# 模拟系统应力-应变关系
for strain in total_strain:
    fiber_stress = E_fibers * strain  # 计算每根纤维的应力
    broken_fibers = fiber_stress > fiber_strength  # 判断纤维是否破坏
    
    # 将破坏的纤维应力置为0，未破坏的纤维保留应力
    fiber_stress[broken_fibers] = 0
    remaining_fibers = np.logical_not(broken_fibers)  # 剩余未破坏的纤维
    
    if np.sum(remaining_fibers) > 0:
        # 重新分配应力：将应力分配给未破坏的纤维
        redistributed_stress = redistribute_stress(remaining_fibers, np.sum(fiber_stress))
        fiber_stress[remaining_fibers] = redistributed_stress
    
    # 计算系统的总应力
    total_stress = np.mean(fiber_stress)
    stress_response.append(total_stress)

# 绘制应力-应变曲线
plt.plot(total_strain, stress_response, label="FBM Stress-Strain Response")
plt.xlabel("Strain")
plt.ylabel("Stress")
plt.title("Fiber Bundle Model (FBM) Stress-Strain Curve")
plt.legend()
plt.grid(True)
plt.show()

