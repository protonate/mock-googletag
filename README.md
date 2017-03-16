# mock-googletag
A basic mock of the googletag object to use in tests if you're testing that sort of thing.

Dependencies are Sinon, Faker and a random number generator utility function (not included). 

```
import faker from 'faker';
import { randomFive } from './fixtures';

var Slot = function Slot({ code, divId }) {
  code = code || `ad-slot-code-${randomFive()}`;
  divId = divId || `div-id-${randomFive()}`;

  var slot = {
    targeting: [],
    getSlotElementId: function getSlotElementId() {
      return divId;
    },

    getAdUnitPath: function getAdUnitPath() {
      return code;
    },

    setTargeting: function setTargeting(key, value) {
      var obj = [];
      obj[key] = value;
      this.targeting.push(obj);
    },

    getTargeting: function getTargeting() {
      return this.targeting;
    },

    getTargetingKeys: function getTargetingKeys() {
      return [];
    },

    clearTargeting: function clearTargeting() {
      return window.googletag.pubads().getSlots();
    }
  };
  slot.spySetTargeting = sinon.spy(slot, 'setTargeting');
  return slot;
};

export function makeSlot() {
  const slot = new Slot(...arguments);
  window.googletag._slots.push(slot);
  return slot;
}

window.googletag = {
  _slots: [],
  pubads: function () {
    var self = this;
    return {
      getSlots: function () {
        return self._slots;
      },

      setSlots: function (slots) {
        self._slots = slots;
      }
    };
  }
};

```
