# shares distributions and quicks 

Gamma Finance
https://github.com/CodeHawks-Contests/2025-02-gamma/blob/84b9da452fc84762378481fa39b4087b10bab5e0/contracts/PerpetualVault.sol#L755C4-L788C4

```
 function _mint(uint256 depositId, uint256 amount, bool refundFee, MarketPrices memory prices) internal {
    uint256 _shares;
    if (totalShares == 0) {
      _shares = depositInfo[depositId].amount * 1e8;
    } else {
      uint256 totalAmountBefore;
      if (positionIsClosed == false && _isLongOneLeverage(beenLong)) {
        totalAmountBefore = IERC20(indexToken).balanceOf(address(this)) - amount;
      } else {
        totalAmountBefore = _totalAmount(prices) - amount;
      }
      if (totalAmountBefore == 0) totalAmountBefore = 1;
      _shares = amount * totalShares / totalAmountBefore;
    }

    depositInfo[depositId].shares = _shares;
    totalShares = totalShares + _shares;

    if (refundFee) {
      uint256 usedFee = callbackGasLimit * tx.gasprice;
      if (depositInfo[counter].executionFee > usedFee) {
        try IGmxProxy(gmxProxy).refundExecutionFee(depositInfo[counter].owner, depositInfo[counter].executionFee - usedFee) {} catch {}
      }
    }

    emit Minted(depositId, depositInfo[depositId].owner, _shares, amount);
  }
```
