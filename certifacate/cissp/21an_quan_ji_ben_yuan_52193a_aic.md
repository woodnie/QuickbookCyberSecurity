### 2.1安全基本原则:AIC {#2-1-aic}

2.1安全的基本原则

AIC:

         Availability 可用性:保护确保授权用户能够对数据和资源进行及时可靠的访问。信息系统应该提供充分的功能，从而能够在可以接受的性能级别以可预计的方式运行。它们应该能够以一种安全而快速的方式从崩溃中恢复，这样生产活动就不会受到负面的影响。

        控制措施：

                        RAID

                        Clustering

                        Load blancing

                        Redundanty data and power lines (cables)

                        Software and data backup

                        Disk shadowing

                        Co-location and off site facilitues

                        Roll-back functions

                        Fail-over configurations

         Integrity 完整性: 保证信息和系统的准确性和可靠性，并禁止对数据的非授权更改。硬件，软件和通信机制必须协同工作，以便正确地维护和处理数据，并且能够在不被意外更改的情况下将数据移动至预期的目的地。应当保护系统和网络免受外界的干扰和污染。

        控制：

                Hashing(data integrity)

                Configuration managemanet(system interity)

                Change Control (process integrity)

                Access Control (physical and technical)

                Software digital signing

                Transmission CRC function

         Confidentiality 机密性:确保在数据处理的每一个交叉点上都实施了必要级别的安全保护并且阻止未经授权的信息披漏。在数据存储到网络内部的系统和设备上时、数据传输以及数据到达目的地后，这种级别的机密性都应该发挥作用。

        攻击行为：网络监控，肩窥（shouder surfing）,盗取密码文件，社会工程；

        控制：存储和传输数据时进行加密，严格的访问控制，数据分类，人员培训进行数据保护措施培训。

                Encryption fro data at rest(whole disk,database encrytion)

                Encryption for data in transit (IPSec,SSL,PPTP,SSH)

                Access Control (physical and technical)

                Data classification

                Training personnel on the proper data protection procedures