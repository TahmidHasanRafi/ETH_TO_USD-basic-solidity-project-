// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;
// solhint-disable-next-line interface-starts-with-i
interface AggregatorV3Interface {
  function decimals() external view returns (uint8);

  function description() external view returns (string memory);

  function version() external view returns (uint256);

  function getRoundData(
    uint80 _roundId
  ) external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);

  function latestRoundData()
    external
    view
    returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);
}
contract ETHtoUSD{
    uint256 public minimumSendUSD= 2e18;
    function converter() public payable{ 
        require(conversionrate (msg.value)>=minimumSendUSD,"Not Enough ETH Send");
    }
    function getPrice() public view returns(uint256) {
        AggregatorV3Interface pricefeed = AggregatorV3Interface(0x694AA1769357215DE4FAC081bf1f309aDC325306);
        (,int256 price,,,)= pricefeed.latestRoundData();
        return uint256(price * 1e10);      
    }
    function conversionrate(uint256 ethamount) public view returns(uint256){
        uint256 ethPrice = getPrice();
        uint256 ethamountInusd = (ethPrice * ethamount) / 1e18;
        return ethamountInusd;
    }
    function getVersion() public view returns (uint256){
        return AggregatorV3Interface(0x694AA1769357215DE4FAC081bf1f309aDC325306).version();
    }
}
